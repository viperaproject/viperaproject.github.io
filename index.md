The **Vipera** framework is a compact, efficient and high-performance framework for delivering dynamic languages, such as **Python**, on micro-core architectures. 

**Vipera** includes support for running dynamic languages in a _custom virtual machine_ and compilation to a _native code abstract machine_, both specifically designed for the tiny amounts of on-chip memory (32 – 64KB) found on micro-core devices. The **Olympus** native code generation framework includes a _plug-in_ compiler toolset and a compact runtime, supporting dynamic code loading on micro-core accelerators. 

The **vPython** programming language, included with **Vipera**, leverages the framework to provide an easy to use, productive, parallel version of the **Python** programming language, specifically designed for micro-core accelerators. Programs can either be run under **vPython** _standalone_ on micro-core devices, or kernels can be _offloaded_ from within **CPython** scripts running on the host.

# Key features:
## Portable 
The **Vipera** framework is supported on a number of micro-core architectures, including: **RISC-V**, **Xilinx MicroBlaze** and **Adapteva Epiphany-III**. Written entirely in **C99**, **Vipera** can be ported to other architectures that support a standard C compiler such as **GCC** or **Clang**.
## Compact 
The **Vipera** native code abstract machine and virtual machine only require between **4KB** and **43KB** of code memory and **1-2KB** of data memory.
## Powerful 
**Vipera** provides a rich set of support functions and communication primitives (point-to-point, broadcast, reductions, synchronisation etc.) to support parallel programming on micro-core devices.
## Fast 
**Vipera**’s native code abstract machine approaches (>90%) native **C** performance for scientific kernels, greatly closing the performance gap between kernels written in dynamic languages and those hand-crafted in **C**. 

# Free and Open Source
The **Vipera** framework, including compiler, abstract and virtual machines, and runtime libraries are available for use under the [BSD 3-clause license](https://opensource.org/licenses/BSD-3-Clause).
The **BSD Open Source license** allows you to use and modify **Vipera** for personal, commercial and educational use.
The **Vipera** source code is available on [**GitHub**](https://github.com/viperaproject).
