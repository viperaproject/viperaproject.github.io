---
title: "Adding a new device"
image: 
  path: /images/category-whitespace.png
  thumbnail: /images/category-thumbnail.png
  # caption: "vPython scripts"
icon: /images/processor.png
icon_alt: Processor icon
---
The layered architecture of **Vipera** greatly simplifies the process of adding a support for a new micro-core architecture. The key components that are likely to require modification to support a new device are highlighted in the following diagram. 

![New device changes](/images/Vipera-arch-device-changes-v1.png)

Unless there are specific features of the new device that require additions to existing languages e.g. **vPython**, no modifications are requried to either the **Olympus** native code compiler or the **Vipera** virtual machine compilers. Areas of change include the device support in the **Device control** on the host, the device support in both the **Olympus** abstract machine and **Vipera** / **vPython** virtual machine running on the device and possibly the underlying physical transport of the **Communications** component if the device is significantly different from existing supported devices. However, the abstract machine, virtual machine and comms library themselves should not require modification as they already abstract over different micro-core device architectures. 

For embedded devices that do not have _memory hierarchies_, changes may be required within the language compilers to prevent applications using shared memory etc. However, this is a simple change that does not impact the rest of a **Vipera** compiler. Similarly, for example, adding support for vector instuctions of a new RISC-V native code target would only impact the **Olympus** code generator and abstract machine; none of the other phases / components of the native code compiler would require modification. 