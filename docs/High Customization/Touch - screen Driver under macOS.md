## Preface

Recently, I have been thinking about getting the XPS 13 7390 2 - in - 1 or the XPS 13 9300 laptop. These two models are equipped with the highest - version CPUs that can currently be used for Hackintosh, and they are both very beautiful. However, the price is not cheap. After calm analysis, I found that I seemed to only be attracted to their 4K touchscreens. So I thought, why don't I just buy a 4K touch - enabled portable monitor directly? And it's much cheaper. So I actually did this, and this article was born.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16464737072727.webp) 

## Default Situation

macOS does not support touch at the bottom layer by default. So this touchscreen can only be clicked with a single finger by default, just like a mouse. The experience can be said to be very poor. However, the person who sold me this screen is a young lady. She previously used it with a Mac mini. I don't quite understand why she bought such a screen that supports 10 - point touch but used it under the condition that macOS can only operate with a single finger by default.

I seem to understand from the pictures posted by the young lady:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16464743909986.jpeg) 

The interfaces on the left side of this monitor from top to bottom are:

- **Micro - USB**
  - Power supply
- **Type - C**
  - Power supply
- **Type - C**
  - One - cable transmission for screen projection
  - Reverse USB power supply
  - Touch USB protocol
- **Standard HDMI** 
  - Not Micro HDMI
  - Supports 4K 60Hz

It seems from the picture that the young lady has never connected the third interface all the time. So she may have never really used the touchscreen function. No wonder she wanted to sell this monitor.

I've digressed a bit. Let's first take a look at the working situation of this touchscreen under macOS. It can be seen that the touch chip recognized at the bottom layer is ILITEK - TP:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16464769438886.webp)

The ILITEK chip capacitive touch controller was developed to meet the needs of more relaxed business fields. It is the 2511/2302/2312 chip provided by ILITEK in Taiwan. It is compatible with capacitive touch screens ranging from 10.4 to 32 inches and provides an economical touch solution.

## Loading the Touchscreen Driver

When it comes to the macOS touchscreen driver, in fact, our classic I2C touchpad driver also supports touchscreens. The original project address is:

[https://github.com/VoodooI2C/VoodooI2C/](https://github.com/VoodooI2C/VoodooI2C/)

Many classic Hackintosh models, such as the XPS series, also use the drivers of this project.

Without further ado, if our desktop Hackintosh wants to drive the touchscreen, it's also very simple:

1. Place `VoodooI2C.kext` and `VoodooI2CHID.kext` in the `EFI/OC/Kexts` directory.
2. Use OCC to load them in the order shown in the following figure:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16464752859502.webp) 

Then restart. Finally, 10 - point touch can work perfectly under macOS (although macOS doesn't use 10 - point gestures):

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16464755783361.webp) 

However, in this case, single - finger operation of the touchscreen actually simulates a stylus, with the mouse following the finger. Multi - finger operation simulates the Magic Trackpad, and basically all the gestures of the Magic Trackpad are supported:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16464762537969.webp) 

Basically, most of the functions of the Magic Trackpad can be completed.

> For multi - screen operation, because the mouse follows the finger, you can enable "Three - Finger Drag" in the assistive function.

## Driving Method for Genuine Mac

The above operation is very simple through OC. But what if there is no OC in a genuine Mac? In fact, it's also very simple. Just use the command line to manually load the corresponding kexts.

First, download the latest driver version: https://github.com/VoodooI2C/VoodooI2C/releases

Delete other unimportant Kexts and only keep `VoodooI2C.kext` and `VoodooI2CHID.kext` 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16464873056837.webp) 

Next, enter the terminal operation:

```bash
# Modify the owner of kexts
➜  sudo chown -R root:wheel VoodooI2C*

# Add execution permission
➜  sudo chmod -R 755 VoodooI2C*

# Load Kexts
➜  sudo kextload -v VoodooI2C*
```

At this time, a prompt of "System Extension Updated" should pop up:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16464874572041.webp) 

Open "Security & Privacy" in the settings and click "Allow":

![image - 20220305213801994](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16464874834128.webp) 

Then restart the computer according to the prompt.

After restarting, return to the Kexts command and continue to execute the loading command:

```bash
# Load Kexts
➜  sudo kextload -v VoodooI2C*
```

Enter the following command to view the loading status of Kexts:

```bash
➜  VoodooI2C - 2.7 sudo dmesg|grep Voodoo
```

A situation similar to the following figure indicates that the Kexts are successfully loaded:

**...Sorry, the picture is unavailable. Please imagine it yourself...**

But at this time, your touchscreen still doesn't support multi - finger gestures. Let's unplug and replug the USB - protocol data cable and take a look at the log again:

```bash
➜  VoodooI2C - 2.7 sudo dmesg|grep Voodoo
```

It can be seen that the log has changed significantly, and our monitor has been detected:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16464877106505.webp) 

At this time, the multi - finger gestures of your touchscreen monitor should also work normally under the genuine Mac.