---
title: "Adding a new programming language"
image: 
  path: /images/category-whitespace.png
  thumbnail: /images/category-thumbnail.png
  # caption: "vPython scripts"
---

Adding a new programming language to **Vipera** can be as easy as adding a new parser to the **Olympus** native code generation framework. If additional languages features over and above those provided by **Olympus** are required, these can be easily added to the framework. For example, new types can be added to the _Abstract Syntax Tree_ (AST), along with their supporting code generation functions. 

An easy way to start is to look at the **vPython** parser defined in the `vpython.y` Bison grammar file. Here, you will see how the grammar rules map to the functions defined in `vpython.c` that create the respective AST nodes. 

