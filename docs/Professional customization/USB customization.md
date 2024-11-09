## The Significance of USB Customization

If the USB customization is not perfect, the following situations may occur:

1. Bluetooth may not work because Bluetooth uses the USB protocol.
1. Unable to enter the system installation interface normally, with prompts related to the mouse or Magic Trackpad.
2. The system wakes up immediately from sleep because USB devices like Bluetooth are not built - in.
3. USB ports have no response or the speed cannot reach its maximum.
3. **It is recommended to customize the USB before installing the system to avoid unnecessary troubles later.**

## A Little More Explanation

In the early days, the Hackintool was often used to customize USB. However, since macOS 11 and later versions, this method no longer works well. Currently, the most perfect method is to use USBToolBox in Windows to customize the USB, and finally use Hackintool for simple fine - tuning and correction.

## Running the Tool

The official project address of USBToolBox: [https://github.com/USBToolBox/tool/](https://github.com/USBToolBox/tool/)

The compiled download address: [https://github.com/USBToolBox/tool/releases](https://github.com/USBToolBox/tool/releases)

The download address on Lanzouyun: [https://sqlsec.lanzouw.com/iqncozmga8b](https://sqlsec.lanzouw.com/iqncozmga8b)

Download "Windows.exe" to the Windows platform and double - click to run it:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440521534887.webp) 

## Detecting Ports

Input **D** and then press Enter to detect the ports on the computer:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440524912550.webp) 

At this time, the following interface will appear:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440525512069.webp) 

This interface will refresh every 5 seconds.

## Inserting USB Devices

Insert USB 2.0 and USB 3.X devices into each USB port one by one. Stay for 5 seconds each time after insertion. If there are Type - C devices, insert them on both sides (front and back) and record respectively. Finally, the effect of port detection on my laptop is as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440525963508.webp) 

After inserting all the devices one by one, input **B** and press Enter to return to the main menu:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440527625346.webp) 

## Viewing Ports

Return to the main menu and input **S** to view the results of port detection:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16441622517201.webp) 

The results of port detection of my device are as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440528379897.webp) 

It can be seen that these 9 ports 1, 2, 4, 5, 6, 10, 13, 15, and 16 are all active USB ports.

## Exporting Ports

If you feel that there is no problem with the results at this time, input **K** and press Enter to export the `UTBMap.kext` file:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1644052964264.webp) 

Under normal circumstances, it will be saved in the same directory as the current program:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440529273682.webp)

## OC Loading Kexts

Besides the generated `UTBMap.kext` file mentioned above, we also need to use it with `USBToolBox.kext`.

The official download address of USBToolBox.kext: [https://github.com/USBToolBox/kext/releases](https://github.com/USBToolBox/kext/releases)

The download address of version 1.10 on Lanzouyun in China: [https://sqlsec.lanzouw.com/iDh3gzmlxsj](https://sqlsec.lanzouw.com/iDh3gzmlxsj)

Put the two Kexts mentioned above under the Kexts folder of OC and load them. Then remember to uncheck **XhciPortLimit**:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1644055094772.webp) 

Restart to make it effective. So far, your USB has been basically customized, and there will be no problem in normal use. Those with obsessive - compulsive disorder can continue reading.

## Hackintool Verification

Use Hackintool to check and find that each USB port is recognized and working normally:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440576715931.webp) 

However, after inserting devices into each port, it is found that there are indeed more SS02 ports. People without obsessive - compulsive disorder don't need to worry about it. To quote Luo Yonghao's famous saying: It still works, doesn't it?

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440624957394.webp) 

## Hackintool Optimization

Use Hackintool to delete the redundant SS02 ports again, then export USBPorts.kext, load it with OC, and then cancel the enabled status of the two Kexts of USBToolBox previously:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440634206476.webp) 

Restart, and the final effect is as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440635134814.webp) 

Finally, those with obsessive - compulsive disorder will feel comfortable as there are no redundant ports.

!!! note ""
Actually, there are still some flaws. The first HS01 should be a USB2 device. The solution is to use Hackintool to correct and customize it again. But I'm lazy here. It still works, doesn't it?

In this noisy and impetuous era, how many people still insist on writing blogs and outputting original articles? Writing a blog always feels like generating electricity with love...

If you happen to be wealthy and feel that this article has helped you, you can consider making a donation to this article to maintain the high - cost server operation (domain name cost, server cost, CDN cost, etc.)