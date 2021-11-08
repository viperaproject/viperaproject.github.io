---
title: "Vipera architecture"
---

![Vipera architecture](/assets/Vipera_arch_v1d3.png)

The **Vipera** framework consists of a layered architecture with components running on the host and micro-core devices. The framework manages the compilation of code, the transfer and launch of kernels on the micro-core devices, and the transfer of data. With the included **vPython** compiler, **Vipera** supports RISC-V and Xilinx MicroBlaze microcores running on the [Xilinx PYNQ](https://www.xilinx.com/support/university/boards-portfolio/xup-boards.html) single-board computer (SBC), as well as the Adapteva Epiphany-III on the [Adapteva Parallella SBC](https://www.digikey.com/en/product-highlight/a/adapteva/parallella-board). The framework can also support other dynamic languages and micro-core devices through its _plug-in_ architecture. 

# Olympus
The **Olympus** native code generation framework consists of a compiler library, with _pluggable_ language parsers and code generators, and a C-based _abstract machine_ that supports dynamic code loading on micro-core devices. Currently, **Vipera** includes a parser for **vPython** and code generators for the **Olympus** abtsract machine (`olygen`) and the Graphviz DOT graph desciption language (`dotgen`). Olympus can support a number of different micro-cores, including RISC-V, Xilinx MicroBlaze and Adapteva Epiphany, as well as traditional CPUs such as the x86, ARM, SPARC, PowerPC and MIPS. 

The **Olympus** compiler library includes _dynamic type inferencing_ to _infer_ dynamic language variable and function types to enable the generation of optimised _type frozen_ native **Olympus** abstract machine code. In essence, the dynamically-typed **vPython** variable types are _frozen_ until they change type and these frozen types influence the _variants_ of the _polymorphic_ functions generated. This removes the requirement to dynamically type check variables and function dispatch at runtime, greatly increasing the performance of compiled codes. 