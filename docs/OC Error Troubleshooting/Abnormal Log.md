Troubleshooting suggestions: After turning on the `-v` mode, search on Google more often. If you ask others, their brains are not machines. For these repetitive and very similar error messages, except for a few typical ones, it's really hard to directly identify the rest.

Also, if you really have to consult others, at least you should **first configure an EFI by following the tutorial**. Otherwise, if you ask others why there are errors in an EFI you got from somewhere else, it will be very annoying. Because no one knows where you got the EFI from. If the EFI you got is a mess, going to others for troubleshooting will only make both sides more frustrated.

Materials that may be useful for your troubleshooting:

- [【Continuously updated】OpenCore boot - v various stuck situations and a quick - reference table of common problems and solutions for OC boot](http://imacos.top/2021/01/19/0154/)
- [OpenCore configuration errors, faults and solutions (updated on May 2nd)](https://shuiyunxc.github.io/2020/04/06/Faults/index/)

If the troubleshooting doesn't help, and there are still errors when referring to my tutorial, you can consider referring to other people's tutorials or the official OC tutorial:

- [Hackintosh Quick Installation Manual](https://www.yuque.com/hejianzhao/zgnsc5)

## AppleUSBHostPort::createDevice: failed to create device

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1647739675880.webp) This is caused by the conflict between XhciPortLimit and high - version macOS 11.3 +. You can first refer to the [USB customization section](/High Customization/USB customization/) to customize your USB ports, and then disable `XhciPortLimit` under Kernel -> Quirks.

## ACPI Error:[\\\_SB\_.PCI0.XHC\_.RHUB.HS11] Namespace lookup failure

Although there are a large number of errors below, the most critical part is actually the part I circled:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16486487907545.webp) 

Just add a [SSDT - RHUB.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - RHUB.aml) to solve the problem. To fix the problem of some 400 - series motherboards, you need to turn off the RHUB device and force macOS to manually rebuild the ports.

## AppleUSBHostPort::enumeratDeviceComplete_block_invoke

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16487272072775.webp)

This situation indicates a problem with the USB. If changing to other ports doesn't solve the problem, try referring to the [USB customization](/High Customization/USB customization) tutorial to customize in advance.

## ACPI Warning: Unsupported module - level  

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16494861313450.webp)

It seems to be a hard disk problem with apfs_module_start:2568. Actually, the key to this error message is the ACPI error above. I encountered this on a Colorful motherboard. This is because there is a problem with the SSDT. Just find a way to troubleshoot the SSDT.

## com.apple.xpc.launchd

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16501091812562.webp)  

This often appears on some AMD or Intel non - official version CPUs (this is really a pitfall). The solution is to install by faking the CPU. That's it!

## AppleUSBXHCI::creatPorts: unsupported speed mantissa 1248 exponent 2

As the name implies, this gets the USB stuck. I encountered this on a Dell laptop. It seems that all the USB controllers are 3.0, but this will happen when using a USB 2.0 flash drive for installation:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16513781971208.webp) 

Buy a USB 3.0 flash drive, and then refer to the [USB customization tutorial](/Professional customization/USB customization/).