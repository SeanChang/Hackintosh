Device configurations are provided to macOS through dedicated buffers for setting PCI device properties, such as Intel buffer frame patches, sound card Layout ID, etc. Of course, the ID of the sound card can also be added directly in the form of `alcid = xx` through the boot entry.

## Finding Device Addresses

Hardware addresses of different devices are different. You can use Hackintool to view specific address information:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321210545547.webp) 

Or you can also use OCC directly to identify and add device address information:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321212008046.webp) 

The effect after adding is as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321209929227.webp) 

Of course, there are still some missing device properties, which require us to manually supplement them.

## Intel Desktop Platform

### Yonah, Conroe, Penryn, Lynnfield, Clarkdale

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    * This device path is subject to the actual situation and may have errors. You can use Hackintool to obtain the specific device path.
    * layout - id
    * AppleALC audio injection. For the complete ALC ID, you can refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)
    * I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.

These platforms do not require special settings. You only need to add and improve the sound card information. Of course, you can also directly add it through the boot entry. During the early debugging process, I suggest that you add it in the form of `alcid = xx` through the boot entry.

### Sandy Bridge

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321220813709.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    * This device path is subject to the actual situation and may have errors. You can use Hackintool to obtain the specific device path.
    * layout - id
    * AppleALC audio injection. For the complete ALC ID, you can refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)
    * I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.

- **PciRoot(0x0)/Pci(0x2,0x0)**
    * Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    * Configure the iGPU integrated graphics.
    * AAPL,snb - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    * device - id
        - Solve some driver abnormal problems and improve compatibility.


| AAPL,snb - platform - id | Explanation |
| :--------------------- | :----------- |
| **`10000300`**       | When the desktop iGPU is used to drive the output display signal. |
| **`00000500`**       | The integrated graphics is only used for calculation and does not drive the output display signal (recommended when there is a discrete graphics card). |

| device - id      | Explanation |
| :------------- | :----------- |
| **`26010000`** | When the desktop iGPU is used to drive the output display signal. |
| **`02010000`** | The integrated graphics is only used for calculation and does not drive the output display signal (recommended when there is a discrete graphics card). |

The following is an example of the final configuration of the desktop HD 3000 integrated graphics:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321227735696.webp)  

### Ivy Bridge

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321230409173.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    * This device path is subject to the actual situation and may have errors. You can use Hackintool to obtain the specific device path.
    * layout - id
    * AppleALC audio injection. For the complete ALC ID, you can refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)
    * I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.
- **PciRoot(0x0)/Pci(0x2,0x0)**
    * Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    * Configure the iGPU integrated graphics.
    * AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    * device - id
        - Solve some driver abnormal problems and provide compatibility.

| AAPL,ig - platform - id | Explanation |
| :------------------ | :----------- |
| **`0A006601`**      | When the desktop iGPU is used to drive the output display signal. |
| **`07006201`**      | The integrated graphics is only used for calculation and does not drive the output display signal (recommended when there is a discrete graphics card). |

The AAPL,ig - platform - id of the desktop HD 4000 integrated graphics is `0A006601`.

- **PciRoot(0x0)/Pci(0x16,0x0)**
    - Required for the Ivy Bridge CPU to work with 6 - series motherboards (i.e., H61, B65, Q65, P67, H67, Q67, Z68).
    - Deceive the IMEI device to obtain support.
    - This property is still required regardless of whether SSDT - IMEI is used.

| Key       | Type | Value      |
| :-------- | :--- | :--------- |
| device - id | Data | `3A1E0000` |

> If it is a 7 - series motherboard (i.e., B75, Q75, Z75, H77, Q77, Z77), this item is not required.

### Haswell, Broadwell

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321233761618.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    * This device path is subject to the actual situation and may have errors. You can use Hackintool to obtain the specific device path.
    * layout - id
    * AppleALC audio injection. For the complete ALC ID, you can refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)
    * I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.
- **PciRoot(0x0)/Pci(0x2,0x0)**
    * Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    * Configure the iGPU integrated graphics.
    * AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    * device - id
        - Solve some driver abnormal problems and improve compatibility.
    * framebuffer - patch - enable
        - Enable patching through WhateverGreen.kext.
        - If it is discrete graphics output, this property may not be required.
        - If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - stolenmem
        - Set the minimum stolen memory of the integrated graphics.
        - If it is discrete graphics output, this property may not be required.
        - If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - fbmem
        - Set the buffer frame memory size.
        - If it is discrete graphics output, this property may not be required.
        - If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.

| AAPL,ig - platform - id | Explanation |
| :------------------ | :----------- |
| **`0300220D`**      | When the desktop iGPU is used to drive the output display signal. |
| **`04001204`**      | The integrated graphics is only used for calculation and does not drive the output display signal (recommended when there is a discrete graphics card). |
| **`07002216`**      | Another optional ID when the desktop iGPU is used to drive the output display signal. |

| device - id  | Explanation |
| :--------- | :----------- |
| `12040000` | The device ID of the HD 4600 integrated graphics. |

The following is an example of the final configuration of the HD 4400 integrated graphics:

| Key                      | Type | Value      |
| :----------------------- | :--- | :--------- |
| AAPL,ig - platform - id      | Data | `0300220D` |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - stolenmem    | Data | `00003001` |
| framebuffer - fbmem        | Data | `00009000` |
| device - id                | Data | `12040000` |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321314603160.webp)  

The following is an example of the final configuration of the lris Pro 6200 integrated graphics:

| Key                      | Type | Value      |
| :----------------------- | :--- | :--------- |
| AAPL,ig - platform - id      | Data | `07002216` |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - stolenmem    | Data | `00003001` |
| framebuffer - fbmem        | Data | `00009000` |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321314941206.webp)  

### Skylake

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321241655779.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    * This device path is subject to the actual situation and may have errors. You can use Hackintool to obtain the specific device path.
    * layout - id
    * AppleALC audio injection. For the complete ALC ID, you can refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)
    * I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.
- **PciRoot(0x0)/Pci(0x2,0x0)**
    * Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    * Configure the iGPU integrated graphics.
    * AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    * device - id
        - Solve some driver abnormal problems and improve compatibility.
    * framebuffer - patch - enable
        - Enable patching through WhateverGreen.kext.
        - If it is discrete graphics output, this property may not be required.
        - If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - stolenmem
        - Set the minimum stolen memory of the integrated graphics.
        - If it is discrete graphics output, this property may not be required.
        - If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - fbmem
        - Set the buffer frame memory size.
        - If it is discrete graphics output, this property may not be required.
        - If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.

| AAPL,ig - platform - id | Explanation |
| :------------------ | :----------- |
| **`00001219`**      | Used when the desktop iGPU is used to drive the monitor. |
| **`01001219`**      | The integrated graphics is only used for calculation and does not drive the output display signal (recommended when there is a discrete graphics card). |

Users of the HD P530 integrated graphics should note that your iGPU integrated graphics is not natively supported, so you need to add the following properties:

| Key       | Type | Value      |
| :-------- | :--- | :--------- |
| device - id | Data | `1B190000` |

The following is an example of the final configuration of the HD P530 integrated graphics:

| Key                      | Type | Value      |
| :----------------------- | :--- | :--------- |
| AAPL,ig - platform - id      | Data | `00001219` |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - stolenmem    | Data | `00003001` |
| framebuffer - fbmem        | Data | `00009000` |
| device - id                | Data | `1B190000` |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321315379476.webp)  

### Kaby Lake

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321244411668.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    * This device path is subject to the actual situation and may have errors. You can use Hackintool to obtain the specific device path.
    * layout - id
    * AppleALC audio injection. For the complete ALC ID, you can refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)
    * I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.
- **PciRoot(0x0)/Pci(0x2,0x0)**
    * Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    * Configure the iGPU integrated graphics.
    * AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    * device - id
        - Solve some driver abnormal problems and improve compatibility.
    * framebuffer - patch - enable
        - Patch through WhateverGreen.kext.
        - If it is discrete graphics output, this property may not be required.
        - If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - stolenmem
        - Set the minimum stolen memory of the integrated graphics.
        - If it is discrete graphics output, this property may not be required.
        - If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - fbmem
        - Set the buffer frame memory size.
        - If it is discrete graphics output, this property may not be required.
        - If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.

| AAPL,ig - platform - id | Explanation |
| :------------------ | :----------- |
| **`00001259`**      | When the desktop iGPU is used to drive the output display signal. |
| **`03001259`**      | When the desktop iGPU is only used for calculation tasks and does not drive the monitor. |

The following is an example of the final configuration of the HD630 integrated graphics:

| Key                      | Type | Value      |
| :----------------------- | :--- | :--------- |
| AAPL,ig - platform - id      | Data | `00001259` |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - stolenmem    | Data | `00003001` |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321315798117.webp)  

### Coffee Lake

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321321085431.webp)  

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    * This device path is subject to the actual situation and may have errors. You can use Hackintool to obtain the specific device path.
    * layout - id
    * AppleALC audio injection. For the complete ALC ID, you can refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)
    * I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.
- **PciRoot(0x0)/Pci(0x2,0x0)**
    * Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    * Configure the iGPU integrated graphics.
    * AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    * device - id
        - Solve some driver abnormal problems and improve compatibility.
    * framebuffer - patch - enable
        - Enable patching through WhateverGreen.kext. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - stolenmem
        - Set the minimum stolen memory of the integrated graphics. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - fbmem
        - Set the buffer frame memory size. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.

| AAPL,ig - platform - id | Explanation |
| :------------------ | :----------- |
| **`07009B3E`**      | When the desktop iGPU is used to drive the output display signal. |
| **`00009B3E`**      | If `07009B3E` doesn't work, you can consider using this ID. |
| **`0300913E`**      | The integrated graphics is only used for calculation and does not drive the output display signal (recommended when there is a discrete graphics card). |

The following is an example of the final configuration of the HD630 integrated graphics:

| Key                      | Type | Value      |
| :----------------------- | :--- | :--------- |
| AAPL,ig - platform - id      | Data | `07009B3E` |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - stolenmem    | Data | `00003001` |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321323354753.webp) 

### Comet Lake

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321325911237.webp)    

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    * This device path is subject to the actual situation and may have errors. You can use Hackintool to obtain the specific device path.
    * layout - id
    * AppleALC audio injection. For the complete ALC ID, you can refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)
    * I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.

- **PciRoot(0x0)/Pci(0x2,0x0)**
    * Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    * Configure the iGPU integrated graphics.
    * AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    * device - id
        - Solve some driver abnormal problems and improve compatibility.
    * framebuffer - patch - enable
        - Enable patching through WhateverGreen.kext. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - stolenmem
        - Set the minimum stolen memory of the integrated graphics. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - fbmem
        - Set the buffer frame memory size. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.

| AAPL,ig - platform - id | Explanation |
| :------------------ | :----------- |
| **`07009B3E`**      | When the desktop iGPU is used to drive the output display signal. |
| **`00009B3E`**      | If `07009B3E` doesn't work, you can consider using this ID. |
| **`0300C89B`**      | The integrated graphics is only used for calculation and does not drive the output display signal (recommended when there is a discrete graphics card). |

The following is an example of the final configuration of the HD630 integrated graphics:

| Key                      | Type | Value      |
| :----------------------- | :--- | :--------- |
| AAPL,ig - platform - id      | Data | `07009B3E` |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - stolenmem    | Data | `00003001` |

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321323354753.webp) 

## Intel High - end Desktop Platform

### Nehalem, Westmere, Sandy and Ivy Bridge - E, Haswell - E, Skylake - X/W and Cascade Lake - X/W

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    * This device path is subject to the actual situation and may have errors. You can use Hackintool to obtain the specific device path.
    * layout - id
    * AppleALC audio injection. For the complete ALC ID, you can refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)
    * I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.

These platforms do not require special settings. You only need to add and improve the sound card information. Of course, you can also directly add it through the boot entry. During the early debugging process, I suggest that you add it in the form of `alcid = xx` through the boot entry.

## Intel Laptop Platform

### Skylake - X/W, Cascade Lake - X/W

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321331517109.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    * This device path is subject to the actual situation and may have errors. You can use Hackintool to obtain the specific device path.
    * layout - id
    * AppleALC audio injection. For the complete ALC ID, you can refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)
    * I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.
- **PciRoot(0x0)/Pci(0x2,0x0)**
    * Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    * Configure the iGPU integrated graphics.
    * AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    * framebuffer - patch - enable
        - Enable patching through WhateverGreen.kext. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - singlelink
        - Frame buffer single - link. Some old laptops need to configure this option.

| Property                 | Type | Value      |
| :----------------------- | :--- | :--------- |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - singlelink   | Data | `01000000` |

### Sandy Bridge

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321334313962.webp)  

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    * This device path is subject to the actual situation and may have errors. You can use Hackintool to obtain the specific device path.
    * layout - id
    * AppleALC audio injection. For the complete ALC ID, you can refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)
    * I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.

- **PciRoot(0x0)/Pci(0x2,0x0)**
    * Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    * Configure the iGPU integrated graphics.
    * AAPL,snb - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.

| AAPL,snb - platform - id | Type   | Explanation |
| :--------------------- | :----- | :----------- |
| **`00000100`**       | Laptop | For laptops. |
| **`10000300`**       | NUC    | For Intel NUC. |

- **PciRoot(0x0)/Pci(0x16,0x0)**
    * It is a common configuration for Sandy Bridge CPU and Ivy Bridge chipset.
    * Deceive the IMEI device to get support.
    * This property is still required regardless of whether SSDT - IMEI is used.
    * Models with Hx6x chipset need to be configured. You can use AIDA64 to check. For example, the chipset of Core i3 - 3110M is HM67.

| Key       | Type | Value      |
| :-------- | :--- | :--------- |
| device - id | Data | `3A1C0000` |

### Ivy Bridge

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321338062252.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    * This device path is subject to the actual situation and may have errors. You can use Hackintool to obtain the specific device path.
    * layout - id
    * AppleALC audio injection. For the complete ALC ID, you can refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs)
    * I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.
- **PciRoot(0x0)/Pci(0x2,0x0)**
    * Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    * Configure the iGPU integrated graphics.
    * AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    * device - id
        - Solve some driver abnormal problems and improve compatibility.
    * framebuffer - patch - enable
        - Enable patching through WhateverGreen.kext. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - memorycount
        - Match FBMemoryCount.
    * framebuffer - pipecount
        - Match PipeCount.
    * framebuffer - portcount
        - Match PortCount.
    * framebuffer - stolenmem
        - Set the minimum stolen memory of the integrated graphics. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    * framebuffer - con1 - enable
        - Enable patching for external monitor 1.
    * framebuffer - con1 - alldata
        - Connection information (interface information, etc.) of monitor 1.

| AAPL,ig - platform - id | Type   | Explanation |
| :------------------- | :----- | :----------- |
| **`03006601`**      | Laptop | Recommended for monitors with 1366*768 or lower resolution. |
| **`04006601`**      | Laptop | Recommended for monitors with 1600*900 or higher resolution. |
| **`09006601`**      | Laptop | If the above two IDs don't work, you can try this one, mainly for use with some eDP monitors. |
| **`0B006601`**      | NUC    | Recommended for Intel NUC. |

The buffer settings are as follows:

- framebuffer - patch - enable
    - Number
    - 1
- framebuffer - memorycount
    - Number
    - 2
- framebuffer - pipecount
    - Number
    - 2
- framebuffer - portcount
    - Number
    - 4
- framebuffer - stolenmem
    - Data
    - `00000004`
- framebuffer - con1 - enable
    - Number
    - 1
- framebuffer - con1 - alldata
    - Data
    - `02050000 00040000 07040000 03040000 00040000 81000000 04060000 00040000 81000000`

### Haswell

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321348477943.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321348477943.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    - The device path is subject to the actual situation and there may be errors. Hackintool can be used to obtain the specific device path.
    - layout - id
    - AppleALC audio injection. For the complete ALC ID, refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs).
    - I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.

- **PciRoot(0x0)/Pci(0x2,0x0)**
    - Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    - Configure the iGPU integrated graphics.
    - AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    - device - id
        - Solve some driver abnormal problems and improve compatibility.
    - framebuffer - patch - enable
        - Enable patching through WhateverGreen.kext. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    - framebuffer - stolenmem
        - Set the minimum stolen memory of the integrated graphics. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    - framebuffer - fbmem
        - Set the buffer frame memory size. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.

| AAPL,ig - platform - id | Type   | Explanation |
| :------------------- | :----- | :----------- |
| **`0500260A`**      | Laptop | Recommended ID values for HD 5000, HD 5100, and HD 5200 integrated graphics. |
| **`0600260A`**      | Laptop | Recommended ID values for HD 4200, HD 4400, and HD 4600 integrated graphics. Device - id is required. |
| **`0300220D`**      | NUC    | Recommended ID values for all NUCs with the Hasewell architecture. Device - id is required. |

The buffer settings are as follows:

| Key                      | Type | Value      |
| :----------------------- | :--- | :--------- |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - cursormem    | Data | `00009000` |

Device - id values for HD 4200, HD 4400, and HD 4600 integrated graphics:

| Key       | Type | Value      |
| :-------- | :--- | :--------- |
| device - id | Data | `12040000` |

### Broadwell

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321351955596.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321351955596.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    - The device path is subject to the actual situation and there may be errors. Hackintool can be used to obtain the specific device path.
    - layout - id
    - AppleALC audio injection. For the complete ALC ID, refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs).
    - I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.

- **PciRoot(0x0)/Pci(0x2,0x0)**
    - Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    - Configure the iGPU integrated graphics.
    - AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    - device - id
        - Solve some driver abnormal problems and improve compatibility.
    - framebuffer - patch - enable
        - Enable patching through WhateverGreen.kext. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    - framebuffer - stolenmem
        - Set the minimum stolen memory of the integrated graphics. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    - framebuffer - fbmem
        - Set the buffer frame memory size. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.

| AAPL,ig - platform - id | Type   | Explanation |
| :------------------- | :----- | :----------- |
| **`06002616`**      | Laptop | Recommended for most laptops. |
| **`02001616`**      | NUC    | Recommended for NUCs with the Broadwell architecture. |

If your graphics card is HD 5600, you usually need to fake the device - id value:

| Key       | Type | Value    |
| :-------- | :--- | :------- |
| device - id | data | 26160000 |

The buffer settings are as follows:

| Key                      | Type | Value      |
| :----------------------- | :--- | :--------- |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - stolenmem    | Data | `00003001` |
| framebuffer - fbmem        | Data | `00009000` |

### Skylake

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321354577140.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321354577140.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    - The device path is subject to the actual situation and there may be errors. Hackintool can be used to obtain the specific device path.
    - layout - id
    - AppleALC audio injection. For the complete ALC ID, refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs).
    - I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.

- **PciRoot(0x0)/Pci(0x2,0x0)**
    - Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    - Configure the iGPU integrated graphics.
    - AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    - device - id
        - Solve some driver abnormal problems and improve compatibility.
    - framebuffer - patch - enable
        - Enable patching through WhateverGreen.kext. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    - framebuffer - stolenmem
        - Set the minimum stolen memory of the integrated graphics. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    - framebuffer - fbmem
        - Set the buffer frame memory size. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.

| AAPL,ig - platform - id | Type   | Explanation |
| :------------------- | :----- | :----------- |
| **`00001619`**      | Laptop | Recommended for HD 515, HD 520, HD 530, HD 540, HD 550, and P530 integrated graphics. |
| **`00001E19`**      | Laptop | If the above ID doesn't work, you can try this one. |
| **`00001B19`**      | Laptop | Recommended for HD 510. |
| **`00001E19`**      | NUC    | Recommended for HD 515. |
| **`02001619`**      | NUC    | Recommended for HD 520/530. |
| **`02002619`**      | NUC    | Recommended for HD 540/550. |
| **`05003B19`**      | NUC    | Recommended for HD 580. |

If your integrated graphics is HD 510, you usually need to fake the device - id value:

| Key       | Type | Value      |
| :-------- | :--- | :--------- |
| device - id | Data | `02190000` |

If your integrated graphics is HD 550 or P530, you usually need to fake the device - id value:

| Key       | Type | Value      |
| :-------- | :--- | :--------- |
| device - id | Data | `16190000` |

The buffer settings are as follows:

| Key                      | Type | Value      |
| :----------------------- | :--- | :--------- |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - stolenmem    | Data | `00003001` |
| framebuffer - fbmem        | Data | `00009000` |

### Kaby Lake

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321357351587.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321357351587.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    - The device path is subject to the actual situation and there may be errors. Hackintool can be used to obtain the specific device path.
    - layout - id
    - AppleALC audio injection. For the complete ALC ID, refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs).
    - I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.

- **PciRoot(0x0)/Pci(0x2,0x0)**
    - Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    - Configure the iGPU integrated graphics.
    - AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with our system.
    - device - id
        - Solve some driver abnormal problems and improve compatibility.
    - framebuffer - patch - enable
        - Patch through WhateverGreen.kext. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    - framebuffer - stolenmem
        - Set the minimum stolen memory of the integrated graphics. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    - framebuffer - fbmem
        - Set the buffer frame memory size. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.

| AAPL,ig - platform - id | Type   | Explanation |
| :------------------- | :----- | :----------- |
| **`00001B59`**      | Laptop | Recommended for HD 615, HD 620, HD 630, HD 640, and HD 650. |
| **`00001659`**      | Laptop | If `00001B59` fails to accelerate, you can try this value. |
| **`0000C087`**      | Laptop | Recommended for UHD 617 of Amber Lake and UHD 62 of Kaby Lake - R. |
| **`00001E59`**      | NUC    | Recommended for HD 615. |
| **`00001B59`**      | NUC    | Recommended for HD 630. |
| **`02002659`**      | NUC    | Recommended for HD 640/650. |

If your integrated graphics is HD 620, you usually need to fake the device - id value:

| Key       | Type | Value      |
| :-------- | :--- | :--------- |
| device - id | Data | `16590000` |

All HD 6XX series (UHD is fine) may have some small output problems, which may lead to lock - ups or kernel crashes. Consider the following buffer frame patches:

- framebuffer - con1 - enable
    - Data
    - `01000000`
- framebuffer - con1 - alldata
    - Data
    - `01050A00 00080000 87010000 02040A00 00080000 87010000 FF000000 01000000 20000000`

Other buffer frame settings:

| Key                      | Type | Value      |
| :----------------------- | :--- | :--------- |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - stolenmem    | Data | `00003001` |
| framebuffer - fbmem        | Data | `00009000` |

### Coffee Lake, Whiskey Lake

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321428689452.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321428689452.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    - This device path is subject to the actual situation and may have errors. Hackintool can be used to obtain the specific device path.
    - layout - id
    - AppleALC audio injection. For the complete ALC ID, refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs).
    - I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.

- **PciRoot(0x0)/Pci(0x2,0x0)**
    - Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    - Configure the iGPU integrated graphics.
    - AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with the system.
    - device - id
        - Solve some driver abnormal problems and improve compatibility.
    - framebuffer - patch - enable
        - Enable patching through WhateverGreen.kext. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    - framebuffer - stolenmem
        - Set the minimum stolen memory of the integrated graphics. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    - framebuffer - fbmem
        - Set the buffer frame memory size. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.

| AAPL,ig - platform - id | Type   | Explanation |
| :------------------- | :----- | :----------- |
| **`0900A53E`**      | Laptop | Recommended for UHD 630. |
| **`00009B3E`**      | Laptop | Recommended for UHD 620. |
| **`07009B3E`**      | NUC    | Recommended for UHD 620/630. |
| **`0000A53E`**      | NUC    | Recommended for UHD 655. |

For UHD 630, you can try the following device - id to improve stability:

| Key       | Type | Value      |
| :-------- | :--- | :--------- |
| device - id | Data | `9B3E0000` |

For UHD 620 of Coffee Lake CPU, you can try the following device - id to improve stability:

| Key       | Type | Value      |
| :-------- | :--- | :--------- |
| device - id | Data | `9B3E0000` |

The reference settings for other buffer settings are as follows:

| Key                      | Type | Value      |
| :----------------------- | :--- | :--------- |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - stolenmem    | Data | `00003001` |
| framebuffer - fbmem        | Data | `00009000` |

### Coffee Lake Plus and Comet Lake

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321432884424.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321432884424.webp) 

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    - This device path is subject to the actual situation and may have errors. Hackintool can be used to obtain the specific device path.
    - layout - id
    - AppleALC audio injection. For the complete ALC ID, refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs).
    - I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.

- **PciRoot(0x0)/Pci(0x2,0x0)**
    - Basically, the integrated graphics of all Intel devices use this path (the same for genuine Macs).
    - Configure the iGPU integrated graphics.
    - AAPL,ig - platform - id
        - macOS uses this to determine how the iGPU driver interacts with the system.
    - device - id
        - Solve some driver abnormal problems and improve compatibility.
    - framebuffer - patch - enable
        - Enable patching through WhateverGreen.kext. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    - framebuffer - stolenmem
        - Set the minimum stolen memory of the integrated graphics. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.
    - framebuffer - fbmem
        - Set the buffer frame memory size. If it is discrete graphics output, this property may not be required. If the BIOS can set `DVMT Pre - Allocated: 64MB`, this property may not be required either.

| AAPL,ig - platform - id | Type   | Explanation |
| :------------------- | :----- | :----------- |
| **`0900A53E`**      | Laptop | Recommended for UHD 630. |
| **`00009B3E`**      | Laptop | Recommended for UHD 620. |
| **`07009B3E`**      | NUC    | Recommended for UHD 620/630. |
| **`0000A53E`**      | NUC    | Recommended for UHD 655. |

For UHD 630, you can try the following device - id to improve stability:

| Key       | Type | Value      |
| :-------- | :--- | :--------- |
| device - id | Data | `9B3E0000` |

For UHD 620 of Comet Lake CPU, you can try the following device - id to improve stability:

| Key       | Type | Value      |
| :-------- | :--- | :--------- |
| device - id | Data | `9B3E0000` |

The reference settings for other buffer settings are as follows:

| Key                      | Type | Value      |
| :----------------------- | :--- | :--------- |
| framebuffer - patch - enable | Data | `01000000` |
| framebuffer - stolenmem    | Data | `00003001` |
| framebuffer - fbmem        | Data | `00009000` |

## AMD Desktop Platform

Since AMD CPUs do not have integrated graphics, these platforms do not require special settings. You only need to add and improve sound card information. Of course, you can also directly add it through the boot entry. During the early debugging process, I suggest that you add it in the form of `alcid = xx` through the boot entry:

- **PciRoot(0x0)/Pci(0x1b,0x0)**
    - This device path is subject to the actual situation and may have errors. Hackintool can be used to obtain the specific device path.
    - layout - id
    - AppleALC audio injection. For the complete ALC ID, refer to: [AppleALC Supported Device List](https://github.com/acidanthera/AppleALC/wiki/Supported - codecs).
    - I suggest that you directly add it in the form of `alcid = xx` through the boot entry, which is simple and convenient.