Through the configuration of the preparatory work in the previous section, our SSDTs should have been loaded. At this point, generally, we only need to adjust the Quirks. A few SSDTs require corresponding patches to take effect. For details of patching, you can refer to the video content. However, if the following sample configurations require patching, it will be written out and you can just follow it.

## Complete Quirks

Because the versions of different platforms vary, before the detailed explanation, here is a complete Quirks explanation for OC 0.7.3, so that you generally have an idea when setting it up.

- **FadtEnableReset**
    * Fixes reboot and shutdown on old hardware. It's not recommended to enable it unless necessary.
    * Some newer laptops may also need this option to fix reboot and shutdown.
- **NormalizeHeaders**
    * Clears the ACPI header fields. Only macOS 10.13 requires this.
    * Versions after 10.14 have already fixed this problem.
- **RebaseRegions**
    * Tries to tentatively relocate the ACPI memory regions. It must be enabled if using a custom DSDT.
- **ResetHwSig**
    * Applicable to hardware that can't maintain the hardware signature during reboot and causes problems with waking from hibernation.
- **ResetLogoStatus**
    * Hardware that can't display the OEM Windows logo on systems with a BGRT table needs to enable this.
- **SyncTableIds**
    * This can solve the licensing problem in old Windows operating systems caused by the incompatibility between the patched table and the SLIC table.

## Intel Desktop Platform

### Yonah, Conroe, Penryn, Lynnfield, Clarkdale

The initially supported version is OS X 10.4.10, and the last supported operating system version is macOS 10.13.6.

These versions **do not require adjustment of Quirks**, and you can keep them all as the default in the sample configuration.

### Sandy Bridge, Ivy Bridge

The iGPU integrated graphics of Sandy Bridge is only officially supported before macOS 10.13, and most Sandy Bridge motherboards do not support UEFI.

The ACPI of Sandy Bridge and Ivy Bridge often needs to be used with "deletion": delete CpuPm, delete Cpu0Ist.

Fortunately, the default sample file already has these, and we only need to check "Enable" and it's okay.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321016052307.webp) 

These versions **do not require adjustment of Quirks**, and you can keep them all as the default in the sample configuration.

### Haswell, Broadwell, Skylake, Kaby Lake, Coffee Lake, Comet Lake

These versions **do not require adjustment of Quirks**, and you can keep them all as the default in the sample configuration.

## Intel High - end Desktop Platform

### Nehalem, Westmere, Sandy, Ivy Bridge - E, Haswell - E, Broadwell - E, Skylake - X/W, Cascade Lake - X/W

These versions **do not require adjustment of Quirks**, and you can keep them all as the default in the sample configuration.

## Intel Laptop Platform

### Clarksfield, Arrandale

Because of the use of SSDT - XOSI, it also needs to be used with the **Change _OSI to XOSI** patch:

| Comment | String  | Change _OSI to XOSI |
| :------ | :------ | :------------------ |
| Enabled | Boolean | YES                 |
| Count   | Number  | 0                   |
| Limit   | Number  | 0                   |
| Find    | Data    | `5f4f5349`          |
| Replace | Data    | `584f5349`          |

This XOSI patch is also integrated in OCC, and you can patch and enable it with just one click.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321014622683.webp) 

These versions **do not require adjustment of Quirks**, and you can keep them all as the default in the sample configuration.

### Sandy Bridge, Ivy Bridge

The iGPU integrated graphics of Sandy Bridge is only officially supported before macOS 10.13, and most Sandy Bridge motherboards do not support UEFI.

The ACPI of Sandy Bridge and Ivy Bridge often needs to be used with "deletion": delete CpuPm, delete Cpu0Ist.

Fortunately, the default sample file already has these, and we only need to check "Enable" and it's okay.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321016052307.webp) 

Because of the use of SSDT - XOSI, it also needs to be used with the **Change _OSI to XOSI** patch:

| Comment | String  | Change _OSI to XOSI |
| :------ | :------ | :------------------ |
| Enabled | Boolean | YES                 |
| Count   | Number  | 0                   |
| Limit   | Number  | 0                   |
| Find    | Data    | `5f4f5349`          |
| Replace | Data    | `584f5349`          |

This XOSI patch is also integrated in OCC, and you can patch and enable it with just one click.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321014622683.webp) 

These versions **do not require adjustment of Quirks**, and you can keep them all as the default in the sample configuration.

### Haswell, Broadwell, Skylake, Kaby Lake, Coffee Lake, Whiskey Lake, Coffee Lake Plus, Comet Lake, Icelake

Because of the use of SSDT - XOSI, it also needs to be used with the **Change _OSI to XOSI** patch:

| Comment | String  | Change _OSI to XOSI |
| :------ | :------ | :------------------ |
| Enabled | Boolean | YES                 |
| Count   | Number  | 0                   |
| Limit   | Number  | 0                   |
| Find    | Data    | `5f4f5349`          |
| Replace | Data    | `584f5349`          |

This XOSI patch is also integrated in OCC, and you can patch and enable it with just one click.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321014622683.webp) 

These versions **do not require adjustment of Quirks**, and you can keep them all as the default in the sample configuration.

## AMD Desktop Platform

### Bulldozer(15h), Jaguar(16h), Ryzen, Threadripper(17h and 19h)

These versions **do not require adjustment of Quirks**, and you can keep them all as the default in the sample configuration.