---
title: "vPython"
image: 
  path: /images/vPython-scripts.png
  thumbnail: /images/vPython-scripts-thumbnail.png
  caption: "vPython logo"
---

**vPython** is a development of the [**ePython**](https://github.com/mesham/epython) parallel implementation of Python specially designed for micro-core architectures, on top of the **Vipera** framework. **vPython** codes can be executed via both a tiny interpreter (approx. 24KB) and AOT native compilation. 

**vPython** enables Python programmers to easily offload specific kernels in their code onto micro-core accelerators, with the seamless transfer of code and data to the device, as well as the copying back of results from the device. As micro-core devices have very small memory spaces (around 32KB to 64KB), **Vipera** provides the ability to take advantage of the larger, albeit slower, external memories to support usable datasets on the devices, and abstracts over the low-level details required to transfer data on the different devices. 