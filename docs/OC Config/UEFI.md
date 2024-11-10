This section allows different types of UEFI modifications to be applied to the operating system boot loader, mainly the Apple boot loader (boot.efi). These modifications currently provide various patches and environment changes for different firmware types. Some of these features were initially implemented as part of AptioeMoryFix.efi (no longer maintained).

## Complete Quirks

Since the versions of different platforms vary, before the detailed explanation, here is a complete description of the Quirks of OC 0.7.3, so that you generally have an idea when setting them.

- **AllowRelocationBlock**
    * Allows macOS to boot via the relocation block.
    * This quirk requires ProvideCustomSlide to be enabled, and usually AvoidRuntimeDefrag also needs to be enabled to work properly.
    * Although this quirk is required when running older macOS versions on platforms with low memory.
    * It is incompatible with some hardware and Mac OS 11. In this case, EnableSafeModeSlide should be considered.
- **AvoidRuntimeDefrag**
    * Prevents boot.efi runtime memory defragmentation.
    * When enabled, it will fix UEFI's runtime services, such as date, time, NVRAM, and power control.
    * Most types of firmware, except Apple and VMware, require this quirk.
- **DevirtualiseMmio**
    * Removes runtime attributes from certain MMIO regions.
    * Removes the runtime bits of known memory regions to reduce the stolen memory footprint in the memory map.
    * Generally, continuous memory injection is performed with slide = 1, so it is usually not checked unless your machine uses the KASLR method.
- **DisableSingleUser**
    * Disables single - user mode.
    * When enabled, it will prohibit the use of `Cmd + S` and `-s`, making the device closer to a T2 genuine Mac.
- **DisableVariableWrite**
    * Prevents write access to macOS NVRAM.
    * When enabled, it will prohibit NVRAM writes. It needs to be enabled on motherboards such as Z390/HM370 that do not have native macOS - supported NVRAM.
- **DiscardHibernateMap**
    * When enabled, it will reuse the original hibernation memory map. Only some old hardware requires this.
- **EnableSafeModeSlide**
    * When enabled, it will allow the use of the Slide value in safe mode.
    * Whether to use continuous memory injection in safe mode (-x).
- **EnableWriteUnprotector**
    * Allows write access to UEFI runtime service code.
    * When enabled, it will delete the write protection in the CR0 register during execution.
- **ForceExitBootServices**
    * Retry ExitBootServices with a new memory map on failure.
    * When enabled, it will ensure that ExitBootServices can be called successfully even when the MemoryMap changes.
    * Generally, very old motherboards may require this, while new motherboards usually do not.
- **ProtectMemoryRegions**
    * Protects memory regions from being accessed incorrectly.
    * Ensures that the CSM memory region is marked as ACPI NVS to prevent boot.efi or XNU from relocating or using them.
    * AvoidRuntimeDefrag solves a similar problem. It is not checked by default.
    * Some old motherboards may require it to be checked.
- **ProtectSecureBoot**
    * Protects UEFI secure boot variables from being written.
    * Prevents the operating system from writing to UEFI secure boot variables (`db`, `dbx`, `PX`, `KEK`).
    * This option is mainly used to avoid NVRAM problems on Insyde motherboards and MacPro5,1.
- **ProtectUefiServices**
    * Protects UEFI services from being overwritten by the firmware.
    * Used to fix problems on Z390 with DevirtualiseMmio, ProtectCsmRegion, or ShrinkMemoryMap.
    * Non - Z390 motherboards are usually not checked by default.
- **ProvideCustomSlide**
    * Provides a custom KASLR slide when there is insufficient memory.
    * This option forces macOS to use a random, non - conflicting slide value among the available slide values.
    * If there is a conflict in the slide value, this option will force macOS to perform the following operation and use a pseudo - random value.
    * Only needed when encountering `Only N/256 slide values are usable!`
- **RebuildAppleMemoryMap**
    * Rebuilds a macOS - compatible memory map.
    * Generally, this option may be required when booting macOS 10.6 and earlier versions.
- **SetupVirtualMap**
    * Sets virtual memory on SetVirtualAddresses.
    * When enabled, it will fix the SetVirtualAddresses call to a virtual address.
    * Establishes continuous memory for OC through virtual memory and maps it to scattered physical memory.
- **SignalAppleOS**
    * Reports the macOS being loaded through the operating system information of any operating system.
    * Reports the information of other operating systems to the macOS being loaded.
    * Used to enable the iGPU for MacBook in Windows.
- **SyncRuntimePermissions**
    * Updates the memory permissions of the runtime environment.
    * Mainly used for early macOS or Linux/Windows.

## Intel Desktop Platform

### Yonah, Conroe, Penryn

- Traditional Boot Settings

| Quirk                  | Enabled | Comment                             |
| :--------------------- | :------ | :---------------------------------- |
| AvoidRuntimeDefrag     | No      | This quirk may need to be enabled for Big Sur. |
| EnableSafeModeSlide    | No      |                                     |
| EnableWriteUnprotector | No      |                                     |
| ProvideCustomSlide     | No      |                                     |
| RebuildAppleMemoryMap  | Yes     | This is required to boot OS X 10.4 to 10.6. |
| SetupVirtualMap        | No      |                                     |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321060833908.webp) 

- UEFI Boot Settings

| Quirk                 | Enabled | Comment                             |
| :-------------------- | :------ | :---------------------------------- |
| RebuildAppleMemoryMap | Yes     | This is required to boot OS X 10.4 to 10.6. |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321061029025.webp)

### Lynnfield, Clarkdale  

- Traditional Boot Settings

| Quirk                  | Enabled | Comment                             |
| :--------------------- | :------ | :---------------------------------- |
| AvoidRuntimeDefrag     | No      | This quirk may need to be enabled for Big Sur. |
| EnableSafeModeSlide    | No      |                                     |
| EnableWriteUnprotector | No      |                                     |
| ProvideCustomSlide     | No      |                                     |
| RebuildAppleMemoryMap  | Yes     | This is required to boot OS X 10.4 to 10.6. |
| SetupVirtualMap        | No      |                                     |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321062291214.webp) 

- UEFI Boot Settings

| Quirk                 | Enabled | Comment                             |
| :-------------------- | :------ | :---------------------------------- |
| RebuildAppleMemoryMap | Yes     | This is required to boot OS X 10.4 to 10.6. |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321062412103.webp) 

### Sandy Bridge, Ivy Bridge, Haswell, Broadwell, Skylake, Kaby Lake

#### ProperTree

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321063359241.webp) 

#### OpenCore Configurator

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321065437221.webp) 

#### OCAuxiliaryTools

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321066197929.webp)  

### Coffee Lake, Comet Lake

#### ProperTree

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321072014472.webp)  

#### OpenCore Configurator

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321073484350.webp) 

#### OCAuxiliaryTools

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1632107491904.webp)  



## Intel High - end Desktop Platform

### Nehalem, Westmere

- Traditional Boot Settings

| Quirk                  | Enabled | Comment                             |
| :--------------------- | :------ | :---------------------------------- |
| AvoidRuntimeDefrag     | No      | This quirk may need to be enabled for Big Sur. |
| EnableSafeModeSlide    | No      |                                     |
| EnableWriteUnprotector | No      |                                     |
| ProvideCustomSlide     | No      |                                     |
| RebuildAppleMemoryMap  | Yes     | This is required to boot OS X 10.4 to 10.6. |
| SetupVirtualMap        | No      |                                     |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321077011087.webp) 

- UEFI Boot Settings

| Quirk                 | Enabled | Comment                             |
| :-------------------- | :------ | :---------------------------------- |
| RebuildAppleMemoryMap | Yes     | This is required to boot OS X 10.4 to 10.6. |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321076965470.webp) 

### Sandy, Ivy Bridge - E, Haswell - E, Broadwell - E

#### ProperTree

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321079933176.webp) 

#### OpenCore Configurator

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321081081008.webp) 

#### OCAuxiliaryTools

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321081961087.webp)  

### Skylake - X/W, Cascade Lake - X/W

#### ProperTree

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321084961598.webp) 

#### OpenCore Configurator

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321087581689.webp) 

#### OCAuxiliaryTools

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321088712875.webp)  

## Intel Laptop Platform

### Clarksfield, Arrandale

The iGPU of Arrandale is only officially supported before macOS 10.13, and most Clarksfield and Arrandale motherboards do not support UEFI.

- Traditional Boot Settings

| Quirk                  | Enabled | Comment                             |
| :--------------------- | :------ | :---------------------------------- |
| AvoidRuntimeDefrag     | No      | This quirk may need to be enabled for Big Sur. |
| EnableSafeModeSlide    | No      |                                     |
| EnableWriteUnprotector | No      |                                     |
| ProvideCustomSlide     | No      |                                     |
| RebuildAppleMemoryMap  | Yes     | This is required to boot OS X 10.4 to 10.6. |
| SetupVirtualMap        | No      |                                     |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321176223321.webp) 

- UEFI Boot Settings

| Quirk                 | Enabled | Comment                             |
| :-------------------- | :------ | :---------------------------------- |
| RebuildAppleMemoryMap | Yes     | This is required to boot OS X 10.4 to 10.6. |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321176509578.webp) 

### Sandy Bridge, Ivy Bridge, Haswell, Broadwell, Skylake, Kaby Lake

The iGPU of Sandy Bridge is only officially supported before macOS 10.13, and most Sandy motherboards do not support UEFI.

#### ProperTree

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321182024920.webp) 

#### OpenCore Configurator

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321189842144.webp)  

#### OCAuxiliaryTools

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321283519219.webp)  

### Coffee Lake, Whiskey Lake

#### ProperTree

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321191449334.webp)  

#### OpenCore Configurator

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321193784801.webp)  

#### OCAuxiliaryTools

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321284307534.webp)  

### Coffee Lake Plus, Comet Lake

#### ProperTree

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-163211961676.webp)  

#### OpenCore Configurator

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321197029614.webp)  

#### OCAuxiliaryTools

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321285705966.webp)  

### Icelake

#### ProperTree

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321199497990.webp) 

#### OpenCore Configurator

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1632120116520.webp)  

#### OCAuxiliaryTools

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321286464942.webp)  

## AMD Desktop Platform

### Bulldozer(15h), Jaguar(16h)

#### ProperTree

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321089756195.webp) 

#### OpenCore Configurator

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321092206693.webp)  

#### OCAuxiliaryTools

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321093162671.webp)  

### Ryzen, Threadripper(17h and 19h)

#### ProperTree

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321094175471.webp) 

- **DevirtualiseMmio** 
  - Note that TRx40 requires this flag.
- **SetupVirtualMap**
  - Note that B550, A520, and TRx40 boards should have this function disabled.
  - Newer X570 BIOS versions also require this function to be disabled.
  - X470 and B450 with BIOS updates in late 2020 also require this function to be disabled.


#### OpenCore Configurator

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321095427594.webp)  

#### OCAuxiliaryTools

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321097311625.webp)