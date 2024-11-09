## Preface

First of all, it should be noted that this dual - graphics card does not refer to two discrete graphics cards, but rather the integrated graphics and the discrete graphics. Just like in the following picture, you can see that both the integrated graphics and the discrete graphics can display normally:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16446323248620.webp) 

Actually, this dual - graphics card configuration of the desktop computer is not the most standard configuration. However, some netizens are rather curious about how it is achieved. This [question](https://github.com/sqlsec/MSI - MAG - B560M - MORTAR - i7 - 10700/issues/8) has been asked for a long time:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16446322411377.webp) 

Since this is the case, it's better to open a chapter to record and share it. Actually, it's very simple.

## Integrated Graphics for Computing Only

The following is the default strategy usually used in my Hackintosh desktop computer (since HDMI is rarely used, the interface data is not customized):

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447223759902.webp) 

Here, AAPL,ig - platform - id uses `0300C89B`, and there is no signal output (because there is already a discrete graphics card). Moreover, the iMac of the genuine Mac also has this default scheduling strategy by default, that is, the integrated graphics is only for computing and does not drive the display. For the complete AAPL,ig - platform - id of UHD 630, you can refer to the following table:

| AAPL,ig - platform - id | Explanation |
|:----------------------:|:-----------:|
| **`07009B3E`**          | Used when the desktop iGPU is used to drive the display |
| **`00009B3E`**          | If `07009B3E` doesn't work, you can consider using this ID to have a look |
| **`0300C89B`**          | Used when the desktop iGPU is only used for computing tasks and does not drive the display |

- device - id: 9B3E0000 
  - Solve some abnormal driver problems and improve compatibility

It can be seen that in my DeviceProperties, there are not many parameters injected in the graphics card part. This is because my BIOS is set to `DVMT Pre - Allocated: 64MB`, so the framebuffer - patch - enable, framebuffer - stolenmem and framebuffer - fbmem parameters are not needed.

In this case, let's open various software to identify:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16446510483478.webp) 

It can be seen that in the Miscellaneous section and system information of Hackintool, only 1 discrete graphics card is identified, the frequency of the integrated graphics is detected in Intel Power Gadget, and the dual - hardware decoding of Video Proc is normal.

## Let the Integrated Graphics Drive the Display

Since we know that the reason is that the integrated graphics does not drive the display, let's directly change AAPL,ig - platform - id to `07009B3E` and have a try:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447223477291.webp) 

Now let's take a look at the identification situation of each software:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16446525224302.webp) 

It can be seen that except that Hackintool identifies two graphics cards here, there is no change in other software, and only the discrete graphics card can still be seen in the system information.

In addition, it should be added that the two graphics cards can also be identified by using the GeekBench 5 benchmarking software:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16446536248013.webp) 

Although there has been progress, the graphics cards are still not identified in the system information, so the revolution has not yet succeeded, and we still need to work hard.

## Display Discrete Graphics and Integrated Graphics in System Information

Actually, this method is very niche and has little to do with your OC configuration. I finally got the answer to this question in the Yuanjing Forum. The original post address is:

[What is the correct way to perfectly display the discrete graphics and integrated graphics on a desktop computer?](https://bbs.pcbeta.com/viewthread - 1920126 - 1 - 1.html)

Finally, 12F gave the correct answer:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1644722687612.webp) 

Indeed, this actually has something to do with the motherboard. Just set the integrated graphics to have priority display in the motherboard, and now think about why it worked for my roommate? It's because during the mining boom, he used the integrated graphics at first, and the discrete graphics was added later, so the situation of dual - graphics cards occurred purely by coincidence later.

My motherboard is the AsRock Z490 Steel Legend. In my BIOS, only the following two settings need to be enabled:

- 「Advanced」-「Chipset Configuration」-「Primary Graphics Adapter」-「**On - board**」
- 「Advanced」-「Chipset Configuration」-「IGPU Multi - Monitor」-「**Enable**」
  - If it is not enabled, the integrated graphics will be disabled when there is a discrete graphics card.
  - Many Gigabyte B - series motherboards are disabled by default, which is very annoying.

Then let's take a look at the final effect picture:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447231169209.webp) 

Let's take a look at the situation of the integrated graphics:

![=](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447231479471.webp) 

Finally, add a general picture and it's almost done:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16447234089799.webp) 

## A Reminder

I wrote this chapter just out of pure curiosity. Actually, this method is not practical.

When there is a discrete graphics card that doesn't need a driver in a desktop computer, it is not recommended to enable the integrated graphics as the display output. Therefore, it is recommended to set the PCIE discrete graphics as the main display device in the BIOS during installation, and then change to an AAPL,ig - platform - id that does not do display output in OC, because this is the default strategy of the iMac of the genuine Mac. The highest level of Hackintosh is to be close to the genuine Mac. In addition, it will also increase the boot speed and shorten the boot time. Why not do it?