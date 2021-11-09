---
title: "Olympus native code generation framework"
image: 
  # path: /images/vPython-scripts.png
  thumbnail: /images/Olympus_components.png
  # caption: "vPython scripts"
---

The **Olympus** native code generation framework consists of a compiler library, with _pluggable_ language parsers and code generators, and a C-based _abstract machine_ that supports dynamic code loading on micro-core devices. Currently, **Vipera** includes a parser for **vPython** and code generators for the **Olympus** abtsract machine (`olygen`) and the Graphviz DOT graph desciption language (`dotgen`). Olympus can support a number of different micro-cores, including RISC-V, Xilinx MicroBlaze and Adapteva Epiphany, as well as traditional CPUs such as the x86, ARM, SPARC, PowerPC and MIPS. 

{% comment %} 
The **Olympus** compiler library includes _dynamic type inferencing_ to _infer_ dynamic language variable and function types to enable the generation of optimised _type frozen_ native **Olympus** abstract machine code. In essence, the dynamically-typed **vPython** variable types are _frozen_ until they change type and these frozen types influence the _variants_ of the _polymorphic_ functions generated. This removes the requirement to dynamically type check variables and function dispatch at runtime, greatly increasing the performance of compiled codes. 
{% endcomment %}