The last section of config.list has a bit of miscellaneous knowledge, but success is just around the corner. Hold on a little longer.

## Embedded APFS
For installing Big Sur and above systems, the default settings are fine:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321912654814.webp) 

If you need to install High Sierra (10.13) - Catalina (10.15) systems, you need to set both MinDate and MinVersion to `-1`.

## AppleInput
This part doesn't need to be changed. Keep all the settings as default:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321914037071.webp) 

## Boot Audio
Basically, the default settings are also okay:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321914251329.webp) 

I'm used to changing PlayChime to Disabled, that is, turning off the boot audio. This boot audio is the "Duang" sound when a genuine Mac boots. You also need to select the path of your audio device and install the corresponding Drivers for it to take effect. So it's better to just turn it off for simplicity.

## UFFI Drivers
This step is used to load the drivers we placed in previous sections:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321915425694.webp) 

Remember to check the "Connect Drivers" below (the English name is: ConnectDrivers).

## Apple Shortcut - related
This part is usually fine with the default settings:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321916787696.webp) 

## Display Output
This part is usually fine with the default settings:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321917196912.webp) 

## Protocol Override
This part is usually fine with the default settings: 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321929309329.webp) 

## Quirks
First, let's look at the quirks part. Some platforms require modifying the quirks information, but in most cases, the default settings are fine.

- **ActivateHpetSupport**
    - Older motherboards, such as those with the ICH6 chipset, do not have relevant HPET settings, so older motherboards need to enable this.
- **EnableVectorAcceleration**
    - When U supports avx512 or avx, enable avx acceleration in the UEFI interface.
- **DisableSecurityPolicy**
    - Turn off Secure Boot on the motherboard's UEFI.
- **ExitBootServicesDelay**
    - Older motherboards need to be given time to exit (in microseconds).
    - For newer motherboards, just fill in `0`.
    - For older motherboards like Z87pro, fill in `3000000 - 5000000`.
- **ForceOcWriteFlash**
    - Enable writing to flash memory for all OpenCore system variables. Generally, this is not checked.
- **ForgeUefiSupport**
    - When checked, it allows running on hardware with old - version EFI 1.x firmware (such as MacPro5,1) for UEFI 2.x firmware.
- **IgnoreInvalidFlexRatio**
    - If you haven't unlocked `MSR0x194` in the BIOS, that is, haven't unlocked CFG, you must check this.
- **ReleaseUsbOwnership**
    - Most motherboards have the function of automatically releasing USB ownership.
    - If the keyboard and mouse freeze during boot or the USB fails, you can consider checking this.
- **ReloadOptionRoms**
    - Allow reloading NVIDIA GOP Option ROM on older Macs after the firmware version is upgraded. Generally, this is not checked.
- **RequestBootVarRouting**
    - Enable the option of the boot disk.
- **TscSyncTimeout**
    - Help some X99, X299 motherboards enable all - core synchronization, but it's not as professional as some Kexts.
    - This option is intended to replace similar patches like `TSCAdjustReset.kext`. The recommended value is `500000`.
    - But this doesn't work after waking up from sleep, so please fill in the default value `0` and use `TSCAdjustReset.kext` for all - core synchronization.
- **UnblockFsConnect**
    - HP laptops may cause OC to be unable to scan the boot items.
    - If your laptop can't recognize the boot items, you can try to enable this.

### Intel Desktop - Yonah, Conroe, Penryn, Lynnfield, Clarkdale, Sandy Bridge, Ivy Bridge, Haswell, Broadwell

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321894392396.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321895523670.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321896418315.webp) 

### Intel Desktop - Skylake, Kaby Lake, Coffee Lake, Comet Lake

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321898585118.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321899851402.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321900087884.webp) 

### Intel High - end Desktop - Nehalem, Westmere, Sandy and Ivy Bridge - E, Haswell - E, Broadwell - E

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1632190119129.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321895523670.webp)

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321896418315.webp)

### Intel High - end Desktop - Skylake - X/W, Cascade Lake - X/W

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321902547574.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321899851402.webp)

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321900087884.webp)

### Intel Laptop - Clarksfield, Arrandale, Sandy Bridge, Ivy Bridge, Haswell, Broadwell

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321904786486.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321905123099.webp)

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321905867870.webp) 

### Intel Laptop - Skylake, Kaby Lake, Coffee Lake, Whiskey Lake, Coffee Lake Plus, Comet Lake, Icelake

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321907123091.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321907449057.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321908455307.webp) 

### AMD Desktop Series

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321910028007.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321899851402.webp)

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16321900087884.webp)