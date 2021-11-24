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

| Module          | Description                                                                                 |
|-----------------|---------------------------------------------------------------------------------------------|
| `array.py`      | Array creation / management functions, including: shape, copying and flattening             |
| `math.py`       | Math functions, including: `pow()`, `exp()`, `sin()`, `cos()`, `log()`, `ceil()`, `floor()` |
| `memory.py`     | Memory management functions: `free()` and `gc()`                                            |
| `microblaze.py` | Xilinx PYNQ MicroBlaze GPIO and associated functions                                        |
| `parallel.py`   | Communications functions, including: `coreid()`, `send()`, `recv()`, `bcast()`, `reduce()`  |
| `random.py`     | Randomisation functions                                                                     |
| `taskfarm.py`   | Task farm management functions                                                              | 
| `util.py`       | Miscellaneous utility functions                                                             |

The `array.py` module provides native support for multi-dimensional arrays within **vPython**, delivering much faster data access for scientific kernels than the _vectors of vectors_ approach dictated by standard **Python** lists. The `memory.py` module allows programmers to finely tune the memory footprint of their codes, especially important in the extremely limited memory environment of micro-core devices. As the name suggests, the `parallel.py` module provides support for parallel programming, including _blocking_ and _non-blocking_ _poiny-to-point_ send functions, _broadcast_ and _reduction_ functions. This and the `taskfarm.py` module allow applications with sophisticated parallel programming patterns to easily developed and deployed to micro-cores.

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

The following code listing illustrates a simple example, executed standalone on the micro-cores and launched from the command line on the host e.g. `vpython example.py`. In this example, each micro-core will generate a random integer between 0 and 100 and then perform a collective message passing reduction (`reduce`) to determine the maximum random number (due to the `max` operator), which is then displayed by each core.

```python
from parallel import reduce
from random import randint

a = reduce(randint(0,100), "max")
print "The highest random number is " + str(a)
```

The above approach allows simple parallel programming examples to be developed quickly and easily in order to learn the fundamental ideas behind parallelism, using a version of the popular **Python** programming language. With this in mind and due to the memory constraints of the target micro-core architectures, **vPython** implements a subset of Python 2.7, and was initially focussed around the imperative aspects of the code with features such as garbage collection. Over time, this has been extended to include other aspects of the **Python** language, although it still does not provide a complete implementation due to memory space limits. However, using **vPython** in anger, it was clear that there was potential for it to be developed to support _real-world_ applications on micro-cores. This required a more powerful approach to programmer interaction, as not all parts of an application are necessarily suited for offloading to micro-cores. Therefore, an approach where specific functions are marked for offloading as _kernels_ to the micro-cores was required, as described in the next section.

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
