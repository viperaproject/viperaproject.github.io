---
title: "Olympus native code generation framework"
image: 
  path: /images/category-whitespace.png
  thumbnail: /images/category-thumbnail.png
  # caption: "vPython scripts"
---

The **Olympus** native code generation framework consists of a compiler library, with _pluggable_ language parsers and code generators, and a C-based _abstract machine_ that supports dynamic code loading on micro-core devices. Currently, **Vipera** includes a parser for **vPython** and code generators for the **Olympus** abtsract machine (`olygen`) and the Graphviz DOT graph desciption language (`dotgen`). Olympus can support a number of different micro-cores, including RISC-V, Xilinx MicroBlaze and Adapteva Epiphany, as well as traditional CPUs such as the x86, ARM, SPARC, PowerPC and MIPS. 

