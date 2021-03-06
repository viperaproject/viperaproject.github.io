---
title: "Adding a new programming language"
image: 
  path: /images/category-whitespace.png
  thumbnail: /images/category-thumbnail.png
icon: /images/computer.png
icon_alt: Computer icon  
---
Adding a new programming language to **Vipera** can be as easy as adding a new parser to the **Olympus** native code generation framework. If additional languages features over and above those provided by **Olympus** are required, these can be easily added to the framework. For example, new types can be added to the _Abstract Syntax Tree_ (AST), along with their supporting code generation functions as outlined in the next article [_Olympus native code generation framework_](/devzone/olympus/). 

{% include toc %}

# Simple expressions
An easy way to start is to look at the **vPython** parser defined in the `vpython.y` [Bison parser generator](https://www.gnu.org/software/bison/) grammar file. Here, you will see how the grammar rules map to the functions defined in `vpython.c` that create the respective AST nodes. For example, the following **Bison** grammar file snippet for **vPython** calls the `appendWhileStatement()` function declared in `vpython.h`:

```python
# From vpython.y grammar file
| WHILE expression COLON codeblock { $$=appendWhileStatement($2, $4); } 
```
The `appendWhileStatement()` function is simply defined in `vpython.c` as follows:

```c
/* From vpython.c */
ASTexpression* appendWhileStatement(ASTexpression* expression, ASTexpression* block) {
	return ast_makeExpression(AST_WHILE, expression, block);
}
```
As we can see, it just calls the **Olympus** compiler framework function `ast_makeExpression()` with the _operation_ `AST_WHILE`, the test `expression` and the block / body of the loop. Relational expressions are created in a similar manner, with the left-hand side `expression1` and right-hand side `expression2`:

```python
# From vpython.y grammar file
relational_expression
        : additive_expression { $$=$1; }
        | relational_expression GT additive_expression { $$=createGtExpression($1, $3); }
        | relational_expression LT additive_expression { $$=createLtExpression($1, $3); }
        | relational_expression LEQ additive_expression { $$=createLeqExpression($1, $3); }
        | relational_expression GEQ additive_expression { $$=createGeqExpression($1, $3); }
```

```c
/* From vpython.c */
ASTexpression* createGtExpression(ASTexpression* expression1, ASTexpression* expression2) {
	return ast_makeExpression(AST_GT, expression1, expression2);
}

ASTexpression* createLtExpression(ASTexpression* expression1, ASTexpression* expression2) {
	return ast_makeExpression(AST_LT, expression1, expression2);
}

ASTexpression* createLeqExpression(ASTexpression* expression1, ASTexpression* expression2) {
	return ast_makeExpression(AST_LEQ, expression1, expression2);
}

ASTexpression* createGeqExpression(ASTexpression* expression1, ASTexpression* expression2) {
	return ast_makeExpression(AST_GEQ, expression1, expression2);
}
```

# More complex expressions
More complex expression types have specific AST node types and creation functions. For example, the `if: ... else:` statement:

```python
# From vpython.y grammar file
| IF expression COLON codeblock ELSE COLON codeblock { $$=appendIfElseStatement($2, $4, $7); }
```

```c
/* From vpython.c */
ASTexpression* appendIfElseStatement(ASTexpression* expressionContainer, 
                                     ASTexpression* thenBlock, 
                                     ASTexpression* elseBlock) {
  ASTstack conditions;

  INIT_STACK(conditions);
  PUSH(conditions, ast_makeExpression(AST_CONDITION, expressionContainer, thenBlock)); 
  return ast_makeCaseClause(&conditions, elseBlock);
}
```

Here, we create a `AST_CASE` node via the **Olympus** `ast_makeCaseClause()` function, which takes a list (stack) of _conditional expressions_ and a _default_ statement. For **vPython** we only create `if: ... else:` chains as **Python** does not support `case` statements but we use the **Olympus** `case` support with a single condition and a default statement as the `else` clause.
> NOTE: Simple expression grammar rules could directly call the `ast_makeExpression()` function with the relevant `ASToperation` (`AST_ADD`, `AST_GT`, `AST_WHILE` etc.) rather than calling a function in the parser **C** file (e.g. `vpython.c`). However, for **vPython**, this indirection allows the same **Bison** grammar file to be used for both the virtual machine compiler and the **Olympus** native code compiler.

# Typing the AST
In the case of **vPython** the AST nodes are traversed to perform _dynamic type inferencing_ in order to determine the type of a variable. The type inferencer also determines the types of the function arguments and return values, creating _polymorphic_ variants of functions, as required. This enables the generation of optimised _type frozen_ native **Olympus** abstract machine code. In essence, the dynamically-typed **vPython** variable types are frozen until they change type. This removes the requirement to dynamically type check variables and function dispatch at runtime, greatly increasing the performance of compiled codes. 

The **Olympus** compiler also performs a separate pass of the code for optimisation e.g. _constant folding_ and this can be extended, as required.
 

