---
title: "vPython"
image: 
  path: /images/category-whitespace.png
  # thumbnail: /images/vPython-v2-thumbnail.png
  thumbnail: /images/vPython-scripts.png
  # caption: "vPython scripts"
---

**vPython** is a parallel implementation of the **Python** programming language specially designed for micro-core architectures, on top of the **Vipera** framework. **vPython** is development of the [**ePython**](https://github.com/mesham/epython) to include the execution of codes via both a tiny interpreter (less than 44KB) and native compilation. 

**vPython** enables Python programmers to easily offload specific kernels in their code onto micro-core accelerators, with the seamless transfer of code and data to the device, as well as the copying back of results from the device. As micro-core devices have very small memory spaces (around 32KB to 64KB), **Vipera** provides the ability to take advantage of the larger, albeit slower, external memories to support usable datasets on the devices, and abstracts over the low-level details required to transfer data on the different devices. 

## Modules
**vPython** provides a number of modules, including support for parallel programming:

|Module           | Comment                                                                          |
------------------------------------------------------------------------------------------------------
| array.py        | Array creation / management functions, including: shape, copying and flattening  |
| math.py         | Math functions, including: pow(), exp(), sin(), cos(), log(), ceil(), floor()    |
| memory.py       | Memory management functions: free() and gc()                                     |
| microblaze.py   | Xilinx PYNQ MicroBlaze GPIO and associated functions                             |
| parallel.py     | Communications functions, including: coreid(), send(), recv(), bcast(), reduce() |
| random.py       | Randomisation functions                                                          |
| taskfarm.py     | Task farm management functions                                                   | 
| util.py         | Miscellaneous utility functions                                                  |

## Running vPython standalone
When **vPython** is run _standalone_, the scripts are compiled and downloaded to the device for execution directly by the **vPython** environment, which controls execution and manages communications between the device cores and the host. Typing **vpython** on the host without any arguments provides the following information:
```
vPython version 2.0, VM running on the Microblaze
vpython [arguments] filename

Where filename is the source code to execute by default on all cores

Arguments
--------
-c placement   Specify core placement; can be a single id, all, a range (a:b) or a list (a,b,c,d)
-d processes   Specify number of process on the device
-h processes   Specify number of process on the host
-overlay path  Specify explicit path to search for overlay location
-nooverlay     Do not reload overlay bitstream on start (default is to always load)
-t             Display core run timing information
-codecore      Placement code on each core (default up to 4096 bytes length)
-codeshared    Placement code in shared memory (automatic after 4096 bytes in length)
-datashared    Data (arrays and strings) stored in shared memory, storage on core is default
-elf           Use ELF device executable
-srec          Use SREC device executable
-s             Display parse statistics
-pp            Display preprocessed code
-o filename    Write out the compiled byte representation of processed Python code and exits (does not run code)
-l filename    Loads from compiled byte representation of code and runs this
-help          Display this help and quit
```
We can see that we have options to chose which cores will execute the code (_placement_),  restrict the number of cores used on the device or number of threads acting as cores on the host (_processes_), where to place the compiled code and data (_local_  on-chip or _shared_ memory) and write out the compiled bytecode for later execution. For _soft-core_ devices running on the Xilinx PYNQ board, there are options to select which _overlay_ (_bitstream_) is loaded prior to executing the bytecode.

The code and data placement options, coupled with the specification of the number of cores used for execution on the device and host, provide a great deal of flexibility. For example, using the former, codes that are bigger than the available micro-core on-chip memory can still be run from the host shared memory, with the respective performance impact. Similarly, it is possible for codes to process data much larger than the on-chip memory by storing it in the shared memory area, whilst the code itself is executing from on-chip memory. Furthermore, it is possible to simulate bigger or smaller devices by running more processes (threads) on the host to simulate more cores, or to reduce the number of physical cores used in order to simulate a smaller device. 

## Offloading vPython kernels within Python applications
**vPython** also supports offloading kernel functions within the standard **CPython** interpreter running on the host. This method allows Python applications to utilise standard **Python** modules e.g. **Numpy**, whilst offloading functions to micro-core accelerator cores and managing the host / device communications implicitly. This is extremely easy to do, as the follwing example demonstrates:

```python
from epython import offload

@offload
def helloworld(a,b):
  print "Hello World"
  return a+b

print helloworld(10, 20)
```

In this **Python** script, we can see that the `helloworld()` function has been marked for downloading and execution on the micro-core device by the `@offload` _decorator_. Note that the values `10` and `20` are sent to the device, and that the return value (`30`) is sent back to the host implicitly by **vPython**. This makes it trivial to take existing **Python** codes and mark specific functions as kernels for execution on the micro-core devices. Perhaps less obvious is that **vPython** also redirects I/O from the device to the host, peforming the underlying communications and I/O on the host transparently. This makes developing and debugging kernels for micro-cores using **vPython** significantly easier and more productive than using the provided **C**-based software development kits (SDKs) for most micro-core architectures.
