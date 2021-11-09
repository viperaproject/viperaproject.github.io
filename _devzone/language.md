---
title: "Adding a new programming language"
image: 
  path: /images/category-whitespace.png
  thumbnail: /images/category-thumbnail.png
  # caption: "vPython scripts"
---

Adding a new programming language to **Vipera** can be as easy as adding a new parser to the **Olympus** native code generation framework. If additional languages features over and above those provided by **Olympus** are required, these can be easily added to the framework. For example, new types can be added to the _Abstract Syntax Tree_ (AST), along with their supporting code generation functions. 

An easy way to start is to look at the **vPython** parser defined in the `vpython.y` Bison grammar file. Here, you will see how the grammar rules map to the functions defined in `vpython.c` that create the respective AST nodes. In the case of **vPython** these nodes are traversed to _dynamically type inference_ to determine the type of a variable. The type inferencer also determines the argument and return types of a function, creating _polymorphic_ variants of functions, as required. This enables the generation of optimised _type frozen_ native **Olympus** abstract machine code. In essence, the dynamically-typed **vPython** variable types are _frozen_ until they change type. This removes the requirement to dynamically type check variables and function dispatch at runtime, greatly increasing the performance of compiled codes. 

The **Olympus** compiler also performs a separate pass of the code for optimisation e.g. _constant folding_ and this can be extended, as required.

