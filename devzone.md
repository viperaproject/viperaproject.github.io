---
title: "Vipera architecture"
layout: collection
permalink: /devzone/
collection: devzone
entries_layout: grid
---

![Vipera architecture](/images/Vipera_arch_v1d3.png)

The **Vipera** framework consists of a layered architecture with components running on the host and micro-core devices. The framework manages the compilation of code, the transfer and launch of kernels on the micro-core devices, and the transfer of data. With the included **vPython** compiler, **Vipera** supports RISC-V and Xilinx MicroBlaze microcores running on the [Xilinx PYNQ](https://www.xilinx.com/support/university/boards-portfolio/xup-boards.html) single-board computer (SBC), as well as the Adapteva Epiphany-III on the [Adapteva Parallella SBC](https://www.digikey.com/en/product-highlight/a/adapteva/parallella-board). The framework can also support other dynamic languages and micro-core devices through its _plug-in_ architecture. 

The following articles describe how to add support for a new micro-core device or programming language:
