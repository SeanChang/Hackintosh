## Preface

Many friends, after they have tinkered with Hackintosh on their own or paid someone to help install Hackintosh, are not clear about the perfection level of their current Hackintosh. So I have opened this chapter separately to share what I consider to be the perfect Hackintosh.

In addition, if you want to know the performance of various parameters under genuine Mac, you can refer to this chapter [Genuine Mac](/7 - Perfect Hackintosh/7 - 2.html)

## General Part

Both laptops and desktops can refer to this part. Why are laptops and desktops separated? Because it's easy for desktops to be perfect, but laptops have a bunch of various problems. So the laptop part will be listed separately below.

### Intel Power Gadget

It's software produced by Intel officially and is often used to view relevant parameters such as CPU frequency, power consumption, and stability.

The official download address is: [https://www.intel.com/content/www/us/en/developer/articles/tool/power - gadget.html](https://www.intel.com/content/www/us/en/developer/articles/tool/power - gadget.html)

First, look at a screenshot of the software running with the CPU normally driven:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447316337343.webp)

The functions of each module from top to bottom are explained as follows:

- **Power**
  - Displays the real - time power consumption of the CPU.
  - Generally, this part is basically normal for everyone, except for a few cases of improper configuration, which may cause the CPU to be in an overclocked state all the time.
- **Frequency**
  - Displays the real - time frequencies of the CPU and integrated graphics.
  - Mainly pay attention to whether the high and low frequencies are normal, not in an overclocked state all the time, or never reaching the turbo boost frequency.
  - CORE MAX: The maximum frequency. Pay attention to whether this reaches the turbo boost standard of your CPU.
  - CORE AVG: The average frequency.
  - CORE MIN: The minimum frequency. Generally, the minimum frequency should be lower than 1.0 to meet expectations. Laptops should pay particular attention to this point because a low frequency helps with battery life.
  - CORE REQ: The CPU frequency that the system predicts needs to be scheduled. Ideally, the pink - red line of **REQ should match the curve trend of the blue line of AVG**.
  - **GFX AVG**: The actual frequency reached by the integrated graphics. If your integrated graphics are not driven, you won't see this option.
  - **GFX REQ**: The integrated graphics frequency that the system predicts needs to be called. If your integrated graphics are not driven, you won't see this option.

It should be added that the frequencies of GFX AVG and GFX REQ are **meaningless**. Different versions of Intel Power Gadget show greatly varying frequencies, so there is no rule at all. The actual perfect effect depends on your own experience.

------

Also, a reminder that using software like **CPU - S** to view variable frequency gears **has no reference value**. I have compared Hackintosh with genuine Mac before. The number of variable frequency gears of genuine Mac is actually less than that of Hackintosh. So I think that more variable frequency gears are not necessarily better. The highest level of Hackintosh should be infinitely close to genuine Mac.

- **Temperature**
  - The real - time temperature of the CPU.
  - If the desktop has good heat dissipation, the maximum daily use temperature will basically not exceed 50℃.
  - For laptops, the heat generation of Intel CPUs has always been a problem. In my genuine Mac, the temperature often reaches 95℃ +, and I'm already used to it.
- **Utilization**
  - The real - time usage rate of the CPU.
  - This is similar to the result viewed in the task manager under Windows, so it will not be elaborated here.

#### Stress Test

Intel Power Gadget has a built - in stress test, which can easily test the turbo boost situations of the CPU and integrated graphics:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-164467418711.webp)

#### Negative Example

After talking so much, let's look at a screenshot with obvious problems in this part:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447317239081.webp)

There are several problems in the figure, and we will list them one by one below:

1. When testing the integrated graphics, the average frequency CORE AVG of the CPU cannot go up and fails to reach the turbo boost standard. It can be clearly seen that the CPU frequency has dropped.
2. CORE REQ actually predicts a frequency of 3.47Ghz, but only reaches a frequency of 0.40Ghz.

### Hackintool

It can be said to be an essential software for Hackintosh. Download link: [https://github.com/headkaze/Hackintool/releases](https://github.com/headkaze/Hackintool/releases)

Hackintool can show the perfection situation of many drivers. Let's look at them one by one. First, look at the system part, and focus on what I have marked:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1644674388451.webp)

- VDA decoder, this is relatively critical. If it is not perfectly supported, your hardware decoding must be abnormal.
- If it is a desktop, pay attention to whether the information of the integrated graphics can be seen here.
- Also, pay attention to whether the graphics card supports Metal, which is a very important graphics engine under macOS.
- Finally, it's best that there are no??? unrecognizable information in the system interface of Hackintool.

#### Miscellaneous

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447318795376.webp)

- First, pay attention to whether all network cards are built - in.
- Whether the Bluetooth firmware can be displayed normally. If not, please try to re - [USB customization](/High Customization/USB customization/)
- The graphics card model is recognized normally. The reason there is no integrated graphics here is that I used a headless ID, only for calculation. It is also recommended that everyone use it this way.
- Besides the built - in sound card, HDMI audio - related information should also be seen in the audio device.
- There is no problem with the hard disk. Basically, everyone's is normal.

#### USB

There are no extra USB ports, and the USB types are correctly recognized, and the speeds are all normal. At the same time, remember to build - in internal devices such as network cards as Internal:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447322128455.webp)

### VideoProc

The statuses of H264 and HEVC should be available for normal operation:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447329581762.webp)

### Geekbench5

Geekbench5 single - core score: 1302, multi - core score: 8393 points

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447369486899.webp)

This score is slightly lower than that when I ran it on macOS 11.4 Big Sur. At that time, the Geekbench5 single - core score was 1314, and the multi - core score was 8687 points. But this is not a big problem. Refer to the scores of previous Macs. As long as the CPU performs at its due level, it's fine: [Geekbench5 scores of previous Macs](https://browser.geekbench.com/mac - benchmarks)

Sapphire 6600XT Platinum Edition, unlock the power consumption limit, and do a little overclocking. Metal score: 98462 points, OpenCL score: 72542 points:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16448001734556.webp)

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16448034866246.webp)

The GPU score of Geekbench5 has always been unstable with relatively large errors. Everyone doesn't need to pay too much attention to the GPU score. By the way, for AMD graphics card optimization, you can refer to this chapter: [AMD free - drive discrete graphics card optimization](/High Customization/Dual - graphics - card display on a desktop computer/)

### System Information

#### NVMExpress

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16450237272241.webp)

WD Western Digital series works perfectly under TRIM. TRIM is a technology unique to Apple and can increase the lifespan of SSDs. Some hard disk and TRIM compatibility programs are also listed in this [issue](https://github.com/dortania/bugtracker/issues/192):

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16450240171228.webp)

If your SSD is not on the above list, but you're not sure whether it works well under TRIM, how can you judge? Here are some characteristics of poor TRIM operation:

1. Programs open slowly, and there are always long lags.
2. The mouse is prone to show the small rainbow circle.
3. In some cases, it even freezes frequently.

If your boot is fine, then to a large extent, it's the compatibility issue between your SSD and TRIM. At this time, just set 「SetApfsTrimTimout」 in 「Kernel kernel settings」 to 999 to turn off TRIM:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16450244397579.webp)

#### Ethernet

Mainly pay attention to whether the maximum connection speed reaches the specification marked on your network card hardware:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16450246714632.webp)

#### Graphics Card

To be supplemented, no picture is attached for the moment.

For desktops, the most perfect graphical situation is to see one graphics card. This is the default policy configuration of iMac. Regarding some people can see both integrated graphics and discrete graphics, in fact, this is not a standard configuration. Friends interested in this configuration can refer to: [Desktop dual - graphics card display](/High Customization/Dual - graphics - card display on a desktop computer/)

The second controversial point is that there is no **EFI driver version** displayed for the graphics card. Genuine Mac basically shows [this information](/7 - Perfect Hackintosh/7 - 2/#_5).

In the early days of 10.15, injecting EFI driver version and other information of genuine Mac cards for third - party graphics cards did help improve performance. But since Big Sur 11, this method is no longer effective. So we don't need to pay too much attention to whether the EFI driver version is displayed.

#### Bluetooth

There's not much to say about this part. If the Bluetooth is driven, relevant Bluetooth information can be seen here. If your Bluetooth doesn't work properly, it's very likely related to USB customization. You can refer to: [USB customization](/High Customization/USB customization/). If it's still a problem after customization, it may also be related to [macOS 12 Bluetooth](/High Customization/Bluetooth in macOS 12/).

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16450259535481.webp)

#### WiFi

Mainly pay attention to whether AirDrop is supported. There's nothing else to pay special attention to:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16450255985904.webp)

There may also be disputes online about country and region codes. In fact, there is no accurate and authoritative conclusion. I think that as long as it's used normally, the region code doesn't matter. Of course, if you don't like it and have to change it, Broadcom network cards do support modification. The following is the official document:
Add the following parameters to the boot option:
- `brcmfx - country = XX` to change the country code to XX (US, CN, #a,...). It can also be injected through DSDT or Properties → DeviceProperties in the boot loader.

### Sound

Support built - in speakers and HDMI or DP sound output:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16451991088028.webp)

When headphones are plugged in, it should also support automatic switching to the headphone device.

### Microphone

The system - built - in microphone can be recognized normally, and the Siri voice assistant can communicate normally:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1645199330665.webp)

### HiDPi

macOS without HiDPi is soulless. 4K monitors generally have HiDPi enabled by default, and for monitors with lower resolutions, HiDPi needs to be enabled manually. For details of enabling, you can refer to: [Enabling HiDPi](/High Customization/Enable HiDPi/)

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16452004986806.webp)

### Sensor - related

Tencent Lemon Cleaner can be used to easily view the CPU temperature and rotation speed:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16450272093892.webp)

For laptops, it's easy for the rotation speed to be unrecognizable. This is really a hard problem for some laptops, and there's no solution. So I just leave it to chance.

iStat Menus can show more detailed temperature information:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16450273871284.webp)

I don't know why iStat Menus didn't recognize the fan rotation speed here either. But it's not a big problem. One thing to pay attention to here is that the GPU temperature of AMD is also recognized. For those who can't recognize the temperature of the discrete graphics card, you can refer to: [AMD free - drive discrete graphics card optimization](/High Customization/Dual - graphics - card display on a desktop computer/)

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16450275226157.webp)

### AppStore

The AppStore should be able to download apps normally, and the download and update should be smooth:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16450265743921.webp)

The reason for mentioning this here is that in the early days, many people using Clover boot had non - standard configurations, which might lead to a poor AppStore experience. So switch to OC as soon as possible and configure it simply following this tutorial.

### Sensi

Sensi is very convenient for testing the read - write speed of the hard disk, and it also has a nice appearance:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517214247479.webp)

### Blackmagic Disk Speed Test

This software can be directly downloaded from the AppStore and can be used to test whether the read - write speed of your SSD is normal:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16450262776331.webp)

If the read - write speed of your SSD is significantly lower than the nominal specification, it's very likely that your SSD and TRIM have poor support. For specific solutions, you can refer to: [General Part - System Information - NVMExpress](/7 - Perfect Hackintosh/7 - 1.html#系统信息)

### Blackmagic RAW Speed Test

This software can be directly downloaded from the AppStore and is used to test the performance of the CPU and graphics card. The following figure shows the test results of i7 - 10700 and RX 6600XT:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16451102673301.webp)

It can be seen that the shortcoming is my i7 - 10700 CPU, of course, compared with the graphics card. But actually, this CPU is more than enough for my daily productivity.

### Messages

If you also have an iOS device, text messages on your macOS should be able to be sent normally:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16451124427589.webp) 

Similarly, they should also be able to be received normally:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16451125087196.webp) 

If you have an iOS device and the network card is confirmed to be qualified, but you still can't send or receive messages, just configure as follows. First, configure the Messages on macOS as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16451126654893.webp) 

Then configure on the iOS side as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1645112779202.jpeg)    

### iCloud

The peripheral functions related to iCloud should also be able to be used normally:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1625477187781.webp)

### AirDrop

Files can be directly transferred among iOS, iPadOS, and macOS devices. Currently, Broadcom's series of wireless network cards that don't require drivers can achieve this:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16262583708801.webp)  

### Handoff

On macOS, system - level applications of iOS and iPadOS can be directly opened at the lower right corner of the Dock:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16451130891375.webp)  

### Cross - device Copy - and - Paste

Actually, the Universal Control feature after macOS 12.3 is more powerful. Before that, only simple text and the like could be pasted across devices through the clipboard:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16451134047441.webp) 

### Sidecar

This is related to the wireless network card. If the wireless network card doesn't require a driver, Sidecar can theoretically work in wireless mode. Otherwise, it can only work normally in wired mode. The function of Sidecar is to use the iPad as a display for macOS:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-164511083231.webp) 

This way, the idle iPad can be reused. It's nice to use it to run QQ and WeChat.

### Universal Control

macOS has officially supported the Universal Control feature since 12.3. This feature is more practical than Sidecar. You can directly use the keyboard and mouse of macOS to operate your iPad. Just move the mouse over:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16451119667345.webp) 

The difference between this and Sidecar in the settings is that in Universal Control, the iPad and the display are not adjacent, while in Sidecar, they are attached together. This can also be directly connected in the Control Center:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16451109186520.webp)   

It should be mentioned here that the brightness slider of my display can't be used because it's a third - party display. In theory, it can be adjusted when using an Apple - made display. However, don't be discouraged if you have a third - party display. Using [MonitorControl](https://github.com/MonitorControl/MonitorControl/releases/) (an open - source software) can also achieve brightness and volume adjustment of the third - party display, which is very convenient:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16451120346440.webp)  

## Laptop Special Topic

For laptops, many can refer to the general part above. Laptops mainly focus on sleep, battery, touchpad, shortcut keys, and brightness adjustment.

### Touchpad

The touchpad should support multi - finger gestures, and the experience should be smooth:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16459602514182.webp)

### Advanced Touchpad

The touchpad should work in the most efficient GPIO mode, rather than the common polling mode:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16459603111142.webp) 

The existence of gpioPin and gpioIRQ in the above figure is a sign of the GPIO interrupt mode. You can also judge by looking at the dmesg log. For advanced touchpad, you can refer to this article of mine: [Touchpad Interrupt Example](/High Customization/Touch - screen Driver under macOS/) 

### Brightness Adjustment

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16459600012550.webp) 

If your laptop doesn't have the [SSDT - PNLF.aml](https://github.com/dortania/Getting - Started - With - ACPI/blob/master/extra - files/compiled/SSDT - PNLF.aml) SSDT, please install it and then check again.

If it still doesn't work, try adding the following boot parameters:

- `-igfxblr` boot parameter (or `enable - backlight - registers - fix` property) to fix the backlight registers on KBL, CFL, and ICL platforms.

Other boot parameters that may be helpful to you:

- `-igfxbls` boot parameter (or `enable - backlight - smoother` property) to make the brightness transition smoother on IVB + platforms.

### Shortcut Keys

All shortcut keys of the laptop should work normally:

![Brightness Shortcut Keys](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16459604822063.webp) 

![Sound Shortcut Keys](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16459605093086.webp)  

Generally, the [**BrightnessKeys.kext**](https://github.com/acidanthera/BrightnessKeys/releases) laptop brightness shortcut key driver is sufficient. Or, if you have the technical ability, writing your own SSDT customization is also theoretically okay.

### Battery

First, the charging state and non - charging state should be able to switch smoothly, and there should be no indication that the battery is unhealthy:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16459608796183.webp) 

The power usage history should also be normal:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16459615381033.webp) 

There are also 7 options in the battery options:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16459615656568.webp) 

The health and temperature should also be normal when using sensi:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16459616848040.webp) 

However, the information such as the production date here is not complete. But it doesn't mean it can't be used, right?

