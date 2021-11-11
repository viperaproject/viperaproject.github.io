---
title: "Olympus native code generation framework"
image: 
  path: /images/category-whitespace.png
  thumbnail: /images/category-thumbnail.png
  # caption: "vPython scripts"
---

The **Olympus** native code generation framework consists of a compiler library, with _plug-in_ language parsers and code generators, and a C-based _abstract machine_ that supports dynamic code loading on micro-core devices. **Olympus** can support a number of different micro-cores, including RISC-V, Xilinx MicroBlaze and Adapteva Epiphany, as well as traditional CPUs such as the x86, ARM, SPARC, PowerPC and MIPS. 

# Compiler framework
![Compiler framework](/images/olympus-compiler-framework-v1.png)

The **Olympus** compiler framework plug-in architecture allows key components to be easily selected at runtime. Currently, **Vipera** includes a parser for **vPython** and code generators for the **Olympus** abstract machine (`olygen`) and the Graphviz DOT graph desciption language (`dotgen`). It is also possible to implement additional compiler phases that perform additional actions on the Abstract Syntax Tree (AST), such as futher code optimisation. 

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
    (IS_ARRAY(LHS(node)) ? "A" : 
        olygenGetTypeName(TYPE(LHS(node)), AST_INITIAL)),
    LEVEL(LHS(node)), OFFSET(LHS(node)), 
    getRefStringChars(rhsString));  
    
  freeRefString(rhsString);
  
  return makeRefString(target->line);
}
```

The key point to note here is that all generators return a `RefString` which is a _reference-counted_ string to allow **Olympus** to manage memory more efficiently. The framework provides a number of functions to create, concatenate and free `RefStrings`. The **Olympus** `ASTtarget` structure includes a large buffer `target->line` to allow generators to use standard C string handling library functions. The above generator example also shows a number of _convenience_ macros to access elements of the AST e.g. `RHS()`, `IS_ARRAY()` and `OFFSET()`. 
> NOTE: These convenience macros should be used in preference to accessing the `ASTexpression` structure fields directly to allow the underlying implementation to be changed without requiring updates to generator functions etc.

# Abstract Machine
The **Olympus** abstract machine consists of a series of C macros, coupled with runtime functions that include memory management. The abstract machine, as the name suggests, models a machine designed to support dynamic object-oriented programming languages with features such as first-class and anonymous functions (_lambdas_). **Olympus** has been specifically designed to support scientific kernels on micro-core devices with extremely limited on-chip memory (c. 32KB). Therefore, dynamic type checking has been eliminated as far as possible by the _type inferencing_ performed by the compiler framework. 

## Memory addressing model
![Memory map](/images/olympus-memory-model-v1.png)

On the device, the **Olympus** memory map consists of the local on-chip RAM (32KB on the Adapteva Epiphany-III, 64KB on the Xilinx MicroBlaze and up to 128KB on the RISC-V), the shared memory on the host (32MB) and the external memory of the on-host **CPython** heap (up to 1GB on the Adapteva Parallella). By default, all _compound_ object[^objects] types (lists, complex numbers etc.) reside in the in-core _local heap_ but there are options to place objects in the memory of other on-chip cores (not shown on the diagram for clarity), the on-host _shared_ and _external_ heaps. The **Vipera** framework provides the underlying communications functions and module to support access to objects in the **CPython** heap on the host. The **Olympus** addressing model also allows compound objects to be placed on the _stack_, increasing performance by reducing the memory allocation overheads associated with allocating objects in the heap.  

[^objects]: We will refer to all variables (integers, floating point, strings, lists etc.) as _objects_. 