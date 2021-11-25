---
title: "Adding a new device"
image: 
  path: /images/category-whitespace.png
  thumbnail: /images/category-thumbnail.png
  # caption: "vPython scripts"
icon: /images/processor.png
icon_alt: Processor icon
---
The layered and plug-in architecture of **Vipera** greatly simplifies the process of adding a support for a new micro-core device. As **Vipera** already supports a number of different micro-core architectures, the platform specific sections of the framework have been isolated to minimise the impact of device-specific features of a target architecture.

{% include toc %}

The key components that are likely to require modification to support a new device are highlighted in the following diagram. 

![New device changes](/images/Vipera-arch-device-changes-v1.png)

## Areas of change
Unless there are specific features of the new device that require additions to existing languages e.g. **vPython**, no modifications are requried to either the **Olympus** native code compiler or the **Vipera** virtual machine compilers. Areas of change include: the device support in **Device control** on the host, the device support in both the **Olympus** abstract machine and **Vipera** / **vPython** virtual machine running on the device and possibly the underlying physical transport of the **Communications** component, if the device is significantly different from existing supported devices. However, the abstract machine, virtual machine and comms library themselves should not require modification as they already abstract over different micro-core device architectures. 

For embedded devices that do not have _memory hierarchies_, changes may be required within the language compilers to prevent applications using shared memory etc. However, this is a simple change that does not impact the rest of a **Vipera** compiler. Similarly, for example, adding support for vector instuctions of a new RISC-V native code target would only impact the **Olympus** code generator and abstract machine; none of the other phases / components of the native code compiler would require modification. 

## Device specifics
Each micro-core device has its own subdirectory in the `devices` directories of the **Vipera** codebase for custom configuration and device-specific files. 

### vPython
If we look at the **vPython** codebase, we see that we have a directory for each of the supported micro-core devices (**Adapteva Epiphany-III**, **Xilinx MicroBlaze** and **RISC-V**) and one for the host: 

```
devices
   ├── epiphany
   │   ├── epiphany-runtime.c
   │   ├── epiphany-shared.h
   │   ├── linker.ldf
   │   ├── main.c
   │   ├── main.h
   │   └── makefile
   ├── host
   │   └── host-shared.h
   ├── microblaze
   │   ├── main.c
   │   ├── main.h
   │   ├── makefile
   │   ├── microblaze-runtime.c
   │   └── microblaze-shared.h
   └── riscv
       ├── init.S
       ├── libsfloat.a
       ├── mailbox.h
       ├── main.c
       ├── main.h
       ├── makefile
       ├── picorv32.ld
       ├── riscv-runtime.c
       └── riscv-shared.h
```

> NOTE: The device subdirectories each contain their own specific `makefiles` and _linker files_ e.g. `epiphany/linker.ldf` and `riscv/picorv32.ld`, where appropriate. The linker files are required to define the _memory map_ for **vPython** as the devices are _bare-metal_ and do not run an Operating System.

The `<device>-runtime.c` file contains the device-specific function definitions required by **vPython** interpreter and the `<device>-shared.h` file contains the definitions that are shared between the device virtual machine and the device support functions running on the host. The `main.h` and `main.c` files contain the standard **C** `main()` function, tailored to the specifics of each device. For certain devices, such as the PicoRV32 RISC-V soft-core, custom **C** support libraries (e.g. `libsfloat.a`) and processor start-up code (e.g. `init.S`) are required, which are bound to the **vPython** binary.

### Olympus
The **Olympus** native code compiler framework does not currently require device-specific configuration or files. However, the **Olympus** abstract machine _does_ require device-specific definitions within the device support area of the runtime library. For example, the **Olympus** abstract machine is 32 bit for all the currently supported micro-core devices but it can be configured to be a different size e.g. 64 bit on an **x86** host or 16 bit on embedded devices, if required. The definitions are held within `types.h` in the main **Olympus** source directory, which also holds processor-specific _alignment_ requirements and processor _endianness_. **Olympus** currently supports a number of processor architectures (**Epiphany**, **MicroBlaze**, **RISC-V**, **ARM**, **x86**, **MIPS**, **SPARC** and **PowerPC**) and **C** compilers (**GCC**, **Clang**, **Watcom**, **TCC** and **SDCC**).

To ensure cross-platform portability, device support customisation code should use the following **Olympus** pre-defined data types and structures:

| Type  |  Description |
|-------|--------------|
| `Int` | Integer variable |
| `Real` | Floating point variable (single precision) |
| `Bool` | Boolean variable |
| `Char` | Character variable |
| `String` | String variable (includes length) |
| `Array` | Array variable (includes dimensions / shape) |
| `Complex` | Complex number variable |
| `Pntr` | A pointer / reference variable |
| `Object` | An object variable |
| `Proc` | A lambda function variable (takes an `Env` and `Object` parameter)|
| `Frame` | A function's stack frame |
| `Env` | An environment (vector of `Frame`s)
| `Value` | Generic memory location to hold values within a `Frame` |
| `StackControlWord` | An encoded value within the stack frame (size and number of pointers) |
| `Level` | The lexical (scope) level of a variable |
| `Offset` | A variable's offset within a `Frame` |
| `Alignment` | Memory alignment value |
| `Size` | The size of a stack frame |
| `Address` | An address value |
| `ArrayDimCount` | The number of dimensions in an array |
| `ArrayDim` | An array dimension (size) |
| `ArrayIndex` | An array index value |
| `HeapBlockSize` | The size of a block within the **Olympus** abstract machine heap |
| `Memory` | An **Olympus** abstract machine memory map (includes environment, stack and display) |
| `Word` | Processor architecture word value (32 bits on current micro-cores) |
| `HWord` | Half word |
| `DWord` | Double word |
| `ValueDefn` | A **vPython** interpreter variable value (includes type information) |
| `SymbolNode` | A **vPython** interpreter variable symbol table node (ID and value) |

> NOTE: There are a set of associated **C** macros that should be used in preference to directly accessing **Olympus** abstract machine structure fields directly which can be found in the `macros.h` and `memory.h` files in the main **Olympus** source directory. These provide the API into the **Olympus** abstract machine and will minimise the impact of underlying changes to the implementation on customised device support code or libraries.


