## Complete Drivers Explanation

There are some driver files in `OC/Drivers`, and these drivers all end with the.efi suffix. The official default driver files and explanations of OC are as follows:

- **AudioDxe.efi**: It is used to play the "Duang" sound during startup, just like a genuine Mac.
- **CrScreenshotDxe.efi**: The driver for taking screenshots in the OC boot interface. Pressing F10 will save a screenshot of the current interface to the root directory of the EFI partition.
- **HiiDatabase.efi**: It is used to support UEFI font rendering. Generally, it is not needed after the 4th - generation Core.
- **NvmExpressDxe.efi**: It is used to make old motherboards support NVME Express devices. Generally, it is not needed after the 4th - generation Core.
- **OpenCanopy.efi**: The essential driver for using a graphical OC theme.
- **OpenHfsPlus.efi**: The file system driver, which is used to support the recognition of the HFS + disk format.
- **OpenLinuxBoot.efi**: A newly added driver in OC 0.7.3, which is used to boot the Linux system.
- **OpenPartitionDxe.efi**: The partition management driver. It is used to load the DMG image of the old - version macOS.
- **OpenRuntime.efi**: The essential core driver of OC, with relatively powerful functions. Just remember that this is an essential driver.
- **OpenUsbKbDxe.efi**: The USB keyboard driver, which is used to simulate Apple hotkeys and is an equivalent solution to KeySupport.
- **Ps2KeyboardDxe.efi**: The PS/2 keyboard driver. The PS/2 keyboard is so old. I haven't seen it for many years.
- **Ps2MouseDxe.efi**: The PS/2 mouse driver. It is also very old, and I haven't seen it for many years.
- **UsbMouseDxe.efi**: The USB mouse driver. Some virtual machines need to rely on this driver to use the mouse in the boot interface.
- **XhciDxe.efi**: The XHCI USB controller driver. Basically, since the 2nd - generation Core, most firmwares have this driver built - in.

## Commonly - used Drivers

Under normal circumstances, the following two are essential drivers:

- **OpenRuntime.efi**: The essential core driver of OC, with relatively powerful functions. Just remember that this is an essential driver.

- **OpenHfsPlus.efi**: The file system driver, which is used to support the recognition of the HFS + disk format.

Because I have the habit of using a graphical interface for booting and taking screenshots in the boot interface, I also use the following two drivers:

- **OpenCanopy.efi**: The essential driver for using a graphical OC theme.

- **CrScreenshotDxe.efi**: The driver for taking screenshots in the OC boot interface. Pressing F10 will save a screenshot of the current interface to the root directory of the EFI partition.

## Comparison Before and After Using Themes

### Before Using Themes

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16319532063208.webp) 

### After Using Themes

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16318842917381.webp)

Some channels for downloading OC themes:

- [https://dortania.github.io/OpenCanopy - Gallery/blackosx.html](https://dortania.github.io/OpenCanopy - Gallery/blackosx.html)
- [OC Themes - Hackintosh Power](https://www.mfpud.com/opencore/octheme/) 
- [https://github.com/LuckyCrack/OpenCore - Themes](https://github.com/LuckyCrack/OpenCore - Themes)
- [https://github.com/chris1111/My - Simple - OC - Themes](https://github.com/chris1111/My - Simple - OC - Themes)

Theme customization is also very simple. Just prepare your icns icon files. Changing the background and icons by yourself can create a new theme, right? Below, I will show you the effect of my custom - made theme:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16459585249452.webp) 