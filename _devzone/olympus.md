---
title: "Olympus native code generation framework"
image: 
  path: /images/category-whitespace.png
  thumbnail: /images/category-thumbnail.png
  # caption: "vPython scripts"
---

The **Olympus** native code generation framework consists of a compiler library, with _pluggable_ language parsers and code generators, and a C-based _abstract machine_ that supports dynamic code loading on micro-core devices. Currently, **Vipera** includes a parser for **vPython** and code generators for the **Olympus** abstract machine (`olygen`) and the Graphviz DOT graph desciption language (`dotgen`). Olympus can support a number of different micro-cores, including RISC-V, Xilinx MicroBlaze and Adapteva Epiphany, as well as traditional CPUs such as the x86, ARM, SPARC, PowerPC and MIPS. 

# Compiler framework


# Abstract Machine
The **Olympus** abstract machine consists of a series of C macros, coupled with runtime functions that include memory management. The abstract machine, as the name suggests, models a machine designed to support dynamic object-oriented programming languages with features such as first-class and anonymous functions (_lambdas_). **Olympus** has been specifically designed to support scientific kernels on micro-core devices with extremely limited on-chip memory (c. 32KB). Therefore, dynamic type checking has been eliminated as far as possible by the _type inferencing_ performed by the compiler framework. 

## Memory addressing model
![Memory map](/images/olympus-memory-model-v1.png)
On the device, the **Olympus** memory map consists of the local on-chip RAM (32KB on the Adapteva Epiphany-III, 64KB on the Xilinx MicroBlaze and up to 128KB on the RISC-V), the shared memory on the host (32MB) and the external memory of the on-host **CPython** heap (up to 1GB on the Adapteva Parallella). By default, all _compound_ object[^objects] types e.g. lists reside in the in-core _local heap_ but there are options to place objects in the _shared heap_ and _external heap_. The **Vipera** framework provides the underlying communications functions and module to support access to objects in the **CPython** heap on the host. The **Olympus** addressing model also allows compound objects, such as complex numbers, to be placed on the _stack_, increasing performance by reducing the memory allocation overheads associated with allocating objects in the heap.  

[^objects]: We will refer to all variable types (integers, floating point, strings, lists etc.) as _objects_. 