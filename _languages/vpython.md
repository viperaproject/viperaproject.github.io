---
title: "vPython"
image: 
  path: /images/vPython-v1.png
  thumbnail: /images/vPython-v1.png
  caption: "vPython logo"
---

_**vPython**_ is a development of the [**ePython**](https://github.com/mesham/epython) parallel implementation of Python specially designed for micro-core architectures, on top of the **Vipera** framework. _**vPython**_ codes can be executed via both a tiny interpreter (approx. 24KB) and AOT[^1] native compilation. 

_**vPython**_ enables Python programmers to easily offload specific kernels in their code onto micro-core accelerators, with the seamless transfer of code and data to the device, as well as the copying back of results from the device. As micro-core devices have very small memory spaces (around 32KB to 64KB), **Vipera** provides the ability to take advantage of the larger, albeit slower, external memories to support usable datasets on the devices, and abstracts over the low-level details required to transfer data on the different devices. 

[^1]: Ahead-Of-Time