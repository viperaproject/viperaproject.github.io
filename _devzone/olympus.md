---
title: "Olympus native code generation framework"
image: 
  path: /images/category-whitespace.png
  thumbnail: /images/category-thumbnail.png
icon: /images/document.png
icon_alt: Document icon  
---
The **Olympus** native code generation framework consists of a compiler library, with _plug-in_ language parsers and code generators, and a **C**-based _abstract machine_ that supports dynamic code loading on micro-core devices. **Olympus** can support a number of different micro-cores, including **RISC-V**, **Xilinx MicroBlaze** and **Adapteva Epiphany**, as well as traditional CPUs such as the **x86**, **ARM**, **SPARC**, **PowerPC** and **MIPS**. 

{% include toc %}

# Compiler framework
![Compiler framework](/images/olympus-compiler-framework-v1.png)

The **Olympus** compiler framework plug-in architecture allows key components to be easily selected at runtime. Currently, **Vipera** includes a parser for **vPython** and code generators for the **Olympus** abstract machine (`olygen`) and the **Graphviz DOT** graph desciption language (`dotgen`). It is also possible to implement additional compiler phases that perform additional actions on the _Abstract Syntax Tree_ (AST), such as futher code optimisation. 

## Code generators
The code generator plug-ins consist of a set of functions that generate code from specific AST node types. These functions are defined for each code generation _target_ and **Olympus** will generate and traverse the AST, applying each function as required.  Each generator is assigned to a specific function pointer within an `ASTtarget` structure, which is then used by **Olympus** during the code generation phase. The _registration_ of the generators is performed by the target initialisation function e.g. `ast_newOlympusTarget(void)`. The following code snippet from the **Olympus** abstract machine target initialisation function shows the registration of each of the generator functions and the `setFilenames` function used by **Olympus** to create the relevant files for the specific target. For example, for **Olympus** abstract machine code, the `<program name>_oly_main.c` and `<program name>_oly_functions.c` files will be created; the former file contains the main program code and the latter file contains the function code.

```c
target->type = OLYMPUS_CODE;
target->setFilenames = olygenSetFilenames;

target->generators.Program = olygenProgram;
target->generators.Declaration = olygenDecl;
target->generators.Id = olygenId;
target->generators.Literal = olygenLiteral;
target->generators.Assignment = olygenAssign;
target->generators.Lambda = olygenLambda;
target->generators.Beta = olygenBeta;  
target->generators.Case = olygenCase;
target->generators.Condition = olygenCondition;
target->generators.While = olygenWhile;
target->generators.For = olygenFor;
target->generators.Return = olygenReturn;
target->generators.Array = olygenArray;
target->generators.ArrayElementIndex = olygenArrayElementIndex;  
target->generators.ArrayElementUpdate = olygenArrayElementUpdate;  
target->generators.Increment = olygenIncrement;
target->generators.Decrement = olygenDecrement;  
target->generators.Arithmetic = olygenArithmetic;
target->generators.Expression = olygenExpression;
```

Each _generator_ has the following function signature:

```c
typedef RefString (*ASTgenFn)(ASTtarget*, ASTexpression*); 
```

Where `ASTtarget` is the current code generation target and `ASTexpression` is the current node being processed. For example, the following is the **Olympus** abstract machine target generator for an assignment: 

```c
static RefString olygenAssign(ASTtarget *target, ASTexpression *node) {
  RefString rhsString = generateCode(target, RHS(node));

  sprintf(target->line,"ST%s(ADDRF(%d,%d),%s);\n",
    (IS_ARRAY(LHS(node)) ? "A" : olygenGetTypeName(TYPE(LHS(node)), AST_INITIAL)),
    LEVEL(LHS(node)), OFFSET(LHS(node)), 
    getRefStringChars(rhsString));  
    
  freeRefString(rhsString);
  
  return makeRefString(target->line);
}
```

A key point to note here is that all generators return a `RefString` which is a _reference-counted_ string to allow **Olympus** to manage memory more efficiently. The framework provides a number of functions to create, concatenate and free `RefStrings`. The **Olympus** `ASTtarget` structure includes a large buffer `target->line` to allow generators to use standard **C** string handling library functions. The above generator example also shows a number of _convenience_ macros to access elements of the AST e.g. `RHS()`, `IS_ARRAY()` and `OFFSET()`. 
> NOTE: These convenience macros should be used in preference to accessing the `ASTexpression` structure fields directly to allow the underlying implementation to be changed without requiring updates to generator functions etc.

Whilst **Olympus** will traverse the AST via the `generateCode()` function, the generators need to be aware of the AST structure as they may need to traverse the sub-nodes differently in order to generate the correct code. For example, the `olygen` and `dotgen` code generators process the `program` node differently as **Olympus** abstract machine code requires that all the functions are declared at the start of the main code file and that the function declarations themselves are placed in the separate functions code file. However, rather than calling generator functions directly, the code generator should pass control back to **Olympus** by calling the `generateCode()` function to process sub-nodes, as shown in the example above. Generally, `ASTexpression` nodes have a left-hand and right-hand side sub nodes (accesible via the `LHS()` and `RHS()` macros), with specialised nodes such as AST_LAMBDA, AST_WHILE etc. having specific `value` structures as shown in the definition of `ASTexpression` below: 

```c
typedef struct expression {
  ASToperation op;
  ASTtype type;
  char *name;
  ASTid id;
  ASTlevel level;
  ASToffset offset;
  struct expression *parent;
  ASTcount referenced;
  struct expression *next;
  union {
    AST_TYPES
    struct array {struct expression **initialisers, *repeat; ASTcount initc;} Array;
    struct indexes {struct expression **indexes; ASTcount indexc;} Indexes;
    struct expr {struct expression *lhs, *rhs;} Expr;
    struct CaseStatement {struct expression **conditions, *dflt; ASTcount condc;} Case;  
    struct lambda {struct expression **args, *body, *variants; ASTcount argc, size;} Lambda; 
    struct beta {struct expression **args, *func; ASTcount argc; ASTboolean recursive;} Beta; 
    struct ForStatement {struct expression *iterator, *list, *block; ASTcount size;} For;
    struct program {struct expression *functions; ASTcount fcount, size;} Program;
  } value;  
} ASTexpression;
```
The `type` and `op` fields of `ASTexpression` store the node's type (`AST_INTEGER`, `AST_REAL`, `AST_STRING`, `AST_COMPLEX` etc.) and the operation (`AST_ADD`, `AST_ASSIGNMENT`, `AST_WHILE`, `AST_LAMBDA` etc.), respectively. For example, the expression `5 + 6` has a type[^arrays] of `AST_INTEGER` and an operation of `AST_ADD` with a `LHS(node)` value of `5` and a `RHS(node)` value of `6`. 

The **Olympus** compiler will build the AST by appending new nodes to the current node's `next` field. Generally, _statements_ will have `next` nodes but `expressions` will not i.e. AST_ASSIGNMENT nodes will likely have a `next` node but AST_ADD nodes will not. 

# Olympus abstract machine
The **Olympus** abstract machine consists of a series of C _mnemonics_, implemented as macros or functions, and a runtime library that include memory management. The abstract machine, as the name suggests, models a machine designed to support dynamic object-oriented programming languages with features such as first-class and anonymous functions (_lambdas_). **Olympus** has been specifically designed to support scientific kernels on micro-core devices with extremely limited on-chip memory (c. 32KB). Therefore, dynamic type checking has been eliminated as far as possible by the _type inferencing_ performed by the compiler framework. 

## Memory addressing model
As can be seen in the `olygenAssign()` generator function listing above, the **Olympus** abstract machine uses a _lexical level_ and _offset_ addressing model. This consists of an _environment_ of _frames_ referenced via a _display_. Every program object[^objects] is declared at a specific offset within a lexical level. For example, all global objects, are declared within lexical level 0 and a function's local variables would be declared within a scope level greater than or equal to 1[^lambdas]. When the code is generated, these levels are translated into _scope_ levels where the level increases outwards from the current scope i.e. within a function, the local scope level is 0 and, for **vPython** programs, the global scope level is 1. **Olympus** abstract machine code programs access objects via the display, which is updated as scope levels are changed e.g. entry / exit of functions and recursive function calls. This translation, and the use of the display, ensure that local object lookups are faster than global object lookups. The example below shows the environment for a program that is able to leverage nested functions. Here, the inner function has a single parameter and local vector, the outer function has two parameters, a local complex number and a string, and two real numbers are declared in the global scope.

![Environment ](/images/olympus-env-model-v1.png)

The example also highlights that _fixed-sized_ compound objects, such as complex numbers, can also reside in the _stack frame_ (`frame n-1`) as well as in the heap. 
> NOTE: The **Olympus** compiler framework will generate the correct levels and offsets for objects within the AST. The `enterScope()` and `leaveScope()` functions within the parser allow scoping levels to be tailored to the specific needs of a particular programming language. For example, the Bison grammar file rules for a `for` loop could use `enterScope()` and `leaveScope()` to ensure that any new declarations are only in-scope within the `for` loop body, as found in programming languages such as Java. The framework also populates the `ASTexpression id` field with a unique ID number, which can be used over and above the level and offset.

The **Olympus** abstract machine provides two object addressing mnemonics: `ADDRF(level,offset)` and `ADDRL(offset)`, where the former returns a _foreign_ object at `level` and `offset`, and the latter returns a _local_ object at `offset`. In general, the `olygen` code generator will generate `ADDRL()` mnemonics for local variables (level = 0) and will generate `ADDRF()` mnemonics when the level is greater than 0. All object access mnemonics use the address returned by these two mnemonics, as shown in the generated code example below, which declares a new object `z` at offset `5` and sets its value to that of object at offset `1` plus `50`.

```c
DECLI("z",ADDRL(5),LDI(ADDRL(1))+50);
```
The object access mnemonics are typed, with the last character of the mmnemonic name denoting the type e.g. `DECLI` declares an integer object and `DECLR` declares a real (`float`) object. The `olygenGetTypeName(TYPE(LHS(node)), AST_INITIAL))` function call used in the `olygenAssign` generator above returns the correct character representing the type. For example, 'I' for integer, 'R' for real, 'S' for string' and 'B' for boolean types. The same function will also return the **C** type name (`Integer`, `Real`, `String`, `Boolean` etc.) by using `AST_CAPITALISE` as the second argument. All the AST types and mnemonics are defined in the `ast.h` file in the `olympus/compiler/ast` subdirectory and the **Olympus** abstract machine mnemonic are defined in the `mnemonics.h` file in the `olympus/runtime` directory.

### Memory map
![Memory map](/images/olympus-memory-model-v1.png)

On the device, the **Olympus** memory map consists of the local on-chip RAM (32KB on the Adapteva Epiphany-III, 64KB on the Xilinx MicroBlaze and up to 128KB on the RISC-V), the shared memory on the host (32MB) and the external memory of the on-host **CPython** heap (up to 1GB on the Adapteva Parallella). By default, all _compound_ object types (lists, complex numbers etc.) reside in the in-core _local heap_ but there are options to place objects in the memory of other on-chip cores (not shown on the diagram for clarity), the on-host _shared_ and _external_ heaps. The **Vipera** framework provides the underlying communications functions and module to support access to objects in the **CPython** heap on the host. The **Olympus** addressing model also allows compound objects to be placed on the _stack_, increasing performance by reducing the memory allocation overheads associated with allocating objects in the heap.  

[^arrays]: Arrays are encoded within the `ASTexpression type` field supported by the the `SET_ARRAY()`, `IS_ARRAY()` and `GET_TYPE()` macros.
[^objects]: We will refer to all variables (integers, floating point, strings, lists etc.) as _objects_. 
[^lambdas]: Although **vPython** does not currently support _nested functions_ (_inner functions_), the **Olympus** abstract machine has full support for nested and anonymous functions. 