NVRAM part. This part is basically universal, so I also like this chapter very much. In this way, there is no need to consider various models, and it is more comfortable to write.

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321868668123.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321868668123.webp) 

This part usually mainly focuses on the following five options.

## UIScale UI Scaling
UIScale has two selectable values, 01 and 02:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321540437078.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321540437078.webp) 

- 01 indicates the default scaling.
- 02 indicates enabling HiDPi.
    - Generally, if it is used with a native 4K display, the effect will be better.
    - If a non - 4K display uses a script to forcibly enable HiDPi, it is also recommended to use this, so that the boot logo will not change in size.

> Update. This part has been adjusted in later versions of OC. The specific changes are as follows:

In later versions of OC, the UCScale parameter has been moved to the "Display Output" tab under "UEFI Settings". In this way, everyone can manually select the corresponding value:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16455447631547.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16455447631547.webp)

## boot - args Boot Options
It is mainly used to set some boot options and the like:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321541684627.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321541684627.webp) 

Common kernel boot identifier set:

| Boot Identifier | Function |
| :---: | :---: |
| `-amd_no_dgpu_accel` | Disable AMD graphics card hardware acceleration |
| `cpus = #` | Enable `#` CPU cores |
| [`darkwake = 0`](http://www.yekki.me/power - nap - and - darkwake - argument/) | Disable Power Nap |
| `dart = 0` | Disable VT - d |
| `debug = 0x100` | Do not auto - restart when KP occurs |
| `kext - dev - mode = 1` | Enable Kext development mode. Do not use if you are not a developer. |
| `-no_compat_check` | Disable compatibility check |
| `-s` | Single - user mode |
| `slide = #` | Manually set the KASLR slide value to `#` |
| `-v` | Verbose boot code mode |
| `-x` | Safe mode |
| `alcid = x` | Inject AppleALC sound card ID |
| `agdpmod = pikera` | High - end A - cards such as 5700XT need to be used with this parameter, otherwise the screen may be black. |
| `-wegnoegpu` | Used to disable all other GPUs except the integrated Intel iGPU |

## **csr - active - config** SIP - related
Settings for System Integrity Protection (SIP). Generally, it is recommended to use the default.

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321545753177.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321545753177.webp) 

Explanation of relevant values:

- `00000000` - SIP is fully enabled (0x0).
- `03000000` - Disable kext signing (0x1) and file system protection (0x2).
- `FF030000` - Disable all flags in macOS High Sierra (0x3ff).
- `FF070000` - Disable all flags in macOS Mojave and in macOS Catalina (0x7ff), because Apple introduced a value for the executable policy.
- `FF0F0000` - Disable all flags in macOS Big Sur (0xfff), which provides another new flag for authenticated root.

I personally recommend using the following to switch SIP, which is more convenient and efficient:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321547057044.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321547057044.webp) 

## prev - lang:kbd Language Settings
Set the language during system installation here:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321547596494.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321547596494.webp) 

If you find that the system installation language is Russian, most likely the language setting here is wrong. Remember to change it back to the corresponding language. After changing, you need to **reset NVRAM** for it to take effect.

Common languages:

| Language | String | DATA Type |
| :---: | :---: | :---: |
| English | en - US:0 | 656e2d55533a30 |
| Chinese | zh - Hans:0 | 7A682D48616E733A323532 |

## WriteFlash
This option is also checked by default. It means allowing to write flash memory for the added variables. We can keep the default:

![(https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321869266370.webp)](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321869266370.webp)