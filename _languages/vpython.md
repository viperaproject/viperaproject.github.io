---
title: vPython
image: 
  path: /images/category-whitespace.png
  # thumbnail: /images/vPython-v2-thumbnail.png
  thumbnail: /images/vPython-scripts.png
  # caption: "vPython scripts"
---
**vPython** is a parallel implementation of the **Python** programming language specially designed for micro-core architectures, on top of the **Vipera** framework. **vPython** is development of the [**ePython**](https://github.com/mesham/epython) to include the execution of codes via both a tiny interpreter (less than 44KB) and native compilation. 

{% include toc %}

# vPython bytecode compiler and VM
**vPython** enables Python programmers to easily offload specific kernels in their code onto micro-core accelerators, with the seamless transfer of code and data to the device, as well as the copying back of results from the device. As micro-core devices have very small memory spaces (around 32KB to 64KB), **Vipera** provides the ability to take advantage of the larger, albeit slower, external memories to support usable datasets on the devices, and abstracts over the low-level details required to transfer data on the different devices. 

## Installation
TBC

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

The `array.py` module provides native support for multi-dimensional arrays within **vPython**, delivering much faster data access for scientific kernels than the _vectors of vectors_ approach dictated by standard **Python** lists. The `memory.py` module allows programmers to finely tune the memory footprint of their codes, especially important in the extremely limited memory environment of micro-core devices. As the name suggests, the `parallel.py` module provides support for parallel programming, including _blocking_ and _non-blocking_ _point-to-point_ send functions, _broadcast_ and _reduction_ functions. This and the `taskfarm.py` module allow applications with sophisticated parallel programming patterns to easily developed and deployed to micro-cores.

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

### Code placement
We can see that we have options to chose which cores will execute the code (_placement_),  restrict the number of cores used on the device or number of threads acting as cores on the host (_processes_), where to place the compiled code and data (_local_  on-chip or _shared_ memory) and write out the compiled bytecode for later execution. For _soft-core_ devices running on the Xilinx PYNQ board, there are options to select which _overlay_ (_bitstream_) is loaded prior to executing the bytecode.

The code and data placement options, coupled with the specification of the number of cores used for execution on the device and host, provide a great deal of flexibility. For example, using the former, codes that are bigger than the available micro-core on-chip memory can still be run from the host shared memory, with the respective performance impact. Similarly, it is possible for codes to process data much larger than the on-chip memory by storing it in the shared memory area, whilst the code itself is executing from on-chip memory. Furthermore, it is possible to simulate bigger or smaller devices by running more processes (threads) on the host to simulate more cores, or to reduce the number of physical cores used in order to simulate a smaller device. 

### Examples
The following code listing illustrates a simple example, executed standalone on the micro-cores and launched from the command line on the host e.g. `vpython example.py`. In this example, each micro-core will generate a random integer between 0 and 100 and then perform a collective message passing reduction (`reduce`) to determine the maximum random number (due to the `max` operator), which is then displayed by each core:

```python
from parallel import reduce
from random import randint

a = reduce(randint(0,100), "max")
print "The highest random number is " + str(a)
```

The following example demonstrates the point-to-point `send()` and `recv()` _blocking_ communication functions, which means that the cores will not continue until they have either fully sent or fully received a value. Therefore, in order to prevent the cores from waiting forever, the send and receive calls should match i.e. a `send()` should always have a corresponding `recv()` call that _consumes_ the message. The functions take a _core ID_, which can be determined using the `coreid()` function as shown in the example. Here, core 0 sends the value `20` to core 1. Therefore, we must ensure that core 1 calls `recv()` to consume the message from core 0.

```python
import parallel

if coreid()==0:
  send(20, 1)
elif coreid()==1:
  print "Got value "+recv(0)+" from core 0"
```

The above approach allows simple parallel programming examples to be developed quickly and easily in order to learn the fundamental ideas behind parallelism, using a version of the popular **Python** programming language. With this in mind and due to the memory constraints of the target micro-core architectures, **vPython** implements a subset of Python 2.7, and was initially focussed around the imperative aspects of the code with features such as garbage collection. Over time, this has been extended to include other aspects of the **Python** language, although it still does not provide a complete implementation due to memory space limits. However, using **vPython** in anger, it was clear that there was potential for it to be developed to support _real-world_ applications on micro-cores. This required a more powerful approach to programmer interaction, as not all parts of an application are necessarily suited for offloading to micro-cores. Therefore, an approach where specific functions are marked for offloading as _kernels_ to the micro-cores was required, as described in the next section.

## Offloading vPython kernels within Python applications
**vPython** also supports offloading kernel functions within the standard **CPython** interpreter running on the host. This method allows Python applications to utilise standard **Python** modules e.g. **Numpy**, whilst offloading functions to micro-core accelerator cores and managing the host / device communications implicitly. This is extremely easy to do, as the follwing example demonstrates:

```python
from vpython import offload

@offload
def helloworld(a,b):
  print "Hello World"
  return a+b

print helloworld(10, 20)
```

In this **Python** script, we can see that the `helloworld()` function has been marked for downloading and execution on the micro-core device by the `@offload` _decorator_. Note that the values `10` and `20` are sent to the device, and that the return value (`30`) is sent back to the host implicitly by **vPython**. This makes it trivial to take existing **Python** codes and mark specific functions as kernels for execution on the micro-core devices. Perhaps less obvious is that **vPython** also redirects I/O from the device to the host, peforming the underlying communications and I/O on the host transparently. This makes developing and debugging kernels for micro-cores using **vPython** significantly easier and more productive than using the provided **C**-based software development kits (SDKs) for most micro-core architectures.

### Data placement / communications
Whilst the implicit data transfer between the host **Python** script and **vPython** kernels is simple to use, it would be useful to provide the programmer with greater control over the data placement of variables and the transfer of their values. The following example declares the array of integers `a` of size 10 and defines a copy on each of the cores using the `define_on_device(a)` function call. The initial values of `a` are based on the values from 1 to 10 passed implicitly in the `updateA()` function call and the initialised array is passed back to the host using the `copy_from_device()` function call.

```python
from vpython import offload, define_on_device, copy_from_device

a=[0]*10

define_on_device(a)

@offload
def updateA(i):
  from parallel import coreid
  a[i]=i * coreid()

for i in range(10):
  updateA(i)

print copy_from_device("a")
```

**vPython** provides the corresponding `copy_to_device()` function to, as the name suggests, copy the value of a host **Python** variable to the cores. By default, these messaging calls are _blocking_ but they can be made _non-blocking_ by passing `async=True` as an argument. Furthermore, the target can be a single core or a list of cores e.g. `target=15`, `target=[1,3,5]` or `target=range(10)`, simplifying the implementation of certain parallel codes and patterns, where computation and communications overlap. For example, data can be transferred to the cores before the kernel functions are launched.

### Code placement
Not only can data be transferred a set of cores, kernel functions can also be downloaded and run on specific cores. The `@offload` _function decorator_ can be passed arguments that limit what cores the function will be executed on. For example, if we wished to only run the `updateA()` function above on only cores 1, 3 and 5, we would use the following decorator `@offload(target=[1,3,5])`. The `offload` decorator provides other parameters that control the execution of the kernel function:

| Parameter  |  Description |
|------------|--------------|
| `async`    | If set to `True`, the kernel will run in a non-blocking manner where the function call will return a _handler_ of type KernelExecutionHandler immediately, which represents the state of the kernel execution of the micro-cores. Thsi can then be tested to determing the run-state of the kernel (completion etc.), allowing waiting on one or more cores. |
| `auto`     | Where `n` is the number of cores the kernel function should execute on but does not specify the actual placement |
| `all`      | If set to `True`, the kernel will execute on all the available micro-cores (default) |
| `target`   | One or more target cores that the kernel function will execute on |

As **vPython** supports micro-core devices that have independently running cores, the above `@offload` parameters allows the deployment of application patterns that rely on different kernels running on different cores at the same time. Furthermore, these kernels can communicate with the host process or the other kernel functions directly, providing a fully parallel programming environment for **vPython** programs on the target micro-core devices.

# vPython native code compiler (vpyc)
The native code compiler `vpyc` has a number of command line options that control the compilation of **vPython** programs. Invoking `vpyc` with the `-h` or `--help` option provides the following information:
```
vPython native code compiler 0.1.238
Copyright (c) 2015-2022 Nick Brown and Maurice Jamieson

Usage: vpyc [options] file... [options]

vPython options:
  -h, --help             Display this information
  -v, --version          Display compiler version
  -o, --output-file      Binary output file name
  -i, --import-dir       Directory to be added to 'import' file path (VIPERA_MODULES)
  -c, --compile-only     Compile vPython source file(s) to Vipera C source code only
  -mf, --main-file       Select the vPython file to provide the 'main' functionality
  -m, --vipera-module    Vipera module (object file)

C compiler options:
  -g, --debug-info       Include C source debug information in binary
  -cc, --c-compile-only  Compile vPython source to object file(s), but not executable(s)
  -I, --c-include-dir    Directory to be added to C include file path
  -L, --c-library-dir    Directory to be added to C library file path
  -cl, --c-library       C library name e.g. m for -lm (libm)
  -cm, --c-module        C module (object file)
  -cf, --c-flag          Flag to be passed to C compiler e.g. O3 for -O3
  -D, --c-define         Define to be passed to C compiler e.g. DEBUG for -DDEBUG
  -march, --machine-arch Select target CPU architecture (platform specific)
```

The compiler supports separate compilation where **vPython** files can be compiled to standalone modules (`.o` object files) that can then be combined to create a single binary executable using the `-m` or `--vipera-module` compiler options to specify the pre-compiled modules and the `-mf` or `--main-file` options to specify the **vPython** file to be used for the _main_ body of the executable. As **Vipera** modules are **C** object files[^vmodules], they can be collated together to form a _library_ archive file using standard Unix utilities such as `ar`. These can then be linked to **vPython** binaries using the `-cl` or `--c-library` options e.g. `-cl fastcalc` for the library `libfastcalc.a`. 

The `-cf` and `--c-flag` options allow options to be passed through to the **C** compiler e.g. `-cf O3` to instruct the **C** compiler to use the `-O3` optimisation level. When using **C** frameworks, such as **Open MPI**, the `-I` or `--c-include-dir` and `-L` or `--c-include-dir` options are passed through to the **C** compiler to allow it to find the _include_ files and libraries. The related `-c` and `-cc` or `--compile-only` and `--c-compile-only` options instruct either the **vPython** to only generate the **Olympus** abstract machine **C** source file and stop, or for the **C** compiler to compile but not link the executable.

For systems with attached micro-core devices, the `-march` or `--machine-arch` compiler options select the target micro-core device e.g. `epiphany` or `microblaze`. 

> NOTE: The target device options available are dependent on those detected by the configuration and installation script. Ensure that the relevant compilers e.g. `mb-gcc` are in the `PATH` variable, or add them on the command line, when configuring the installation script (`make config`). Additional compilers can be added at any time by ensuring they are in the `PATH` variable and running `make config ; make build ; make install` in the base directory of the **Vipera** project source directory.

### C integration
The Olympus `vpyc` compiler allows the integration of **C** within **vPython** programs. This is achieved though a set of `native` keywords as shown in the following **Open MPI** example:

```python
native include(math)
native include(mpi)

def f(a):
     return (4.0 / (1.0 + a * a))

# MPI constants
MPI_COMM_WORLD = native const(MPI_COMM_WORLD)
MPI_SUM = native const(MPI_SUM)
# Use floats not doubles on 32 bit platforms
element = 0.0
if native(->int) sizeof(element) == 4:
 ELEMENT_SIZE = native const(MPI_FLOAT)
else:
 ELEMENT_SIZE = native const(MPI_DOUBLE)

NINTERVALS = 10000
argc = 0
argv = [ "mpipi" ]

myid = 0
numprocs = 0
i = 0

PI25DT = 3.141592653589793238462643
mypi = 0.0
pi = 0.0
h = 0.0
sum = 0.0
x = 0.0
startwtime = 0.0
endwtime = 0.0

native MPI_Init(&argc, &argv)
native MPI_Comm_size(MPI_COMM_WORLD, &numprocs)
native MPI_Comm_rank(MPI_COMM_WORLD, &myid)

startwtime = native(->float) MPI_Wtime()

h = 1.0 / NINTERVALS
sum = 0.0

i = myid + 1
while (i <= NINTERVALS):
    x = h * (i - 0.5)
    sum += f(x)
    i += numprocs

mypi = h * sum

native MPI_Reduce(&mypi, &pi, 1, ELEMENT_SIZE, MPI_SUM, 0, MPI_COMM_WORLD)

if (myid == 0):
    print("")
    native printf("pi is approximately %.16f, Error is %.16f\n", pi, native(->float) fabs(pi - PI25DT))
    endwtime = native(->float) MPI_Wtime()
    print("numprocs:", numprocs, " wall clock time = ", endwtime - startwtime, "\n")

native MPI_Finalize()
```

The **vPython** `native` keyword calls **C** functions with the ability to inform `vpyc` of the function's return type e.g. `native(->float)` indicates that the following **C** function returns a `float` (real) value. The default return value is `int`, in line with standard **C**. The `native const()` option allows the value of **C** constants and variables to be returned to the **vPython** program. Again, the default type is `int` but this can be overridden as required e.g. `native const(CLOCKS_PER_SEC)->float`.

The **C** integration also provides the `&` operator, similar to the **Python** `id()` function, that returns a _reference_ to a **vPython** object. This allows **vPython** values to be updated by **C** frameworks, as shown for `pi` in the `MPI` example above.

As the **C** compiler 
requires access to the relevant libary _include_ files, **vPython** provides the `native include()` option to pass through file references.

> NOTE: The `native include()` option takes the include file base name e.g. `math` for `math.h`.

**vPython** also provides nascent support for compiler _pragmas_ through the `native pragma ''` option to control compiler code generation and warning options.

> NOTE: This functionality is primarily for internal use within **vPython** modules and has only been extensively tested with **GCC**.



[^vmodules]: With additional information used by the **Olympus** **vPython** compiler.