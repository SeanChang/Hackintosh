I wrote an article about HiDPi a few years ago. Netizens who are interested can go and have a look: [What is HiDPI? And how to enable HiDPI on Hackintosh](https://www.sqlsec.com/2018/09/hidpi.html)

Only the key operations will be written in this tutorial without further ado.

## What is HiDPi?

Briefly speaking, it is a display technology that Apple has been using all the time. It improves the clarity by combining multiple pixels into one pixel. When the display meets the retina standard defined by Apple, HiDPi will be automatically enabled. The entire MBP series has HiDPi enabled. If you buy a 4K monitor yourself, HiDPi is also enabled by default. Or when using a 4K laptop Hackintosh (XPS series), HiDPi is also automatically enabled.

Because HiDPi sacrifices pixels, although it looks clearer than before, the actual apparent resolution will be lower. It is equivalent to sacrificing resolution for clarity. So if a 1080P monitor has HiDPi enabled, the final display effect is close to 720P. From this point of view, a 2K - resolution device is more suitable for HiDPi (neither too high nor too low).

## Before Enabling HiDPi Effect

The effect of ZenBoook 13 under the default condition:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440471235590.webp) 

It can be seen that the font in the picture is rather blurry and doesn't look very comfortable.

## After Enabling HiDPi Effect

After enabling HiDPi on Zenbook 13:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440471653893.webp) 

It can be seen that it is much clearer and looks very refreshing. Although a little resolution is sacrificed, it's worth it!

## How to Enable HiDPi

Actually, enabling HiDPi is not complicated. There are mature ready - to - use tools: **The Github project address of the script**: [GitHub - xzhih/one - key - hidpi: Enable macOS HiDPI](https://github.com/xzhih/one - key - hidpi)

Only one command is needed to enable HiDPi:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/xzhih/one - key - hidpi/master/hidpi.sh)"
```

However, the official project on Github is easily blocked. If there is a network timeout when accessing Github, you can use the following script command that I placed in China:

```bash
sh -c "$(curl -fsSL https://html.sqlsec.com/hidpi.sh)"
```

The domestic script synchronizes with the official Github script regularly. The main code has been transferred to Gitee in China. Basically, it can be used smoothly in China.

## Detail 1 of Script Usage

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-164404753673.webp) 

The above are the options I usually use. Because this is a dual - screen laptop, two monitors are listed. If you connect an external monitor, two monitors will also be shown here. Also, for the monitor ICON selection, everyone can choose according to their own preferences.

## Detail 2 of Script Usage

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440475813360.webp) 

Generally, for a 1080P monitor, either option 1 or 2 for the resolution configuration is fine. Everyone can try it out for themselves.

However, the resolutions of some laptops are rather niche. For example, the Chuwi Corebook X 14 previously had a resolution of 2160x1440.

At this time, you have to calculate the resolution with a calculator yourself. The HiDPi resolutions of this Chuwi laptop are finally:

**1920x1280 1620x1080 1380x920 1080x720**

Among them, the area at 1620x1080 will be relatively large, but the clarity is not as good as 1380x920. Netizens can choose according to their own preferences.

## Settings for Enabling HiDPi

In the Settings - Displays, select **Scaling**, and you can see the effect of the monitor with HiDPi enabled. There are small window icons:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440478171055.webp)

However, some monitors do not have this small window. In this case, there will also be a text reminder of HiDPi:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16441642901658.webp) 

## Settings for Not Enabling HiDPi

If the monitor without HiDPi enabled selects **Scaling**, only various resolutions will be listed in text form (without the HiDPi identifier):

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440479704621.webp) 

## Disadvantages of Enabling HiDPi

Everything has its pros and cons. Besides sacrificing resolution (display area) when enabling HiDPi, it will also increase the burden on the graphics card. If your integrated graphics or CPU itself has weak performance, it will cause obvious lagging.

Some friends will surely ask at this point? Why does it increase the burden on the graphics card? You will understand by looking at the following picture of enabling HiDPi:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16440485575047.webp) 

Because HiDPi sacrifices resolution for clarity, when the UI resolution you set is a bit higher, a larger resolution is necessarily required. The original physical resolution cannot meet this, and at this time the graphics card will simulate and output a higher resolution. This is the reason why HiDPi may cause the system to lag. (Common in integrated graphics. For discrete graphics, enabling it without thinking won't be a problem.)