## Preface

When you see this part of the content, it means that you have already delved quite deeply into Hackintosh and need to customize and modify the drivers yourself. So, here, it's assumed that you are already an advanced user, and I won't go over some basic steps in detail.

Actually, this content is taken from this article on my blog: [Research on Touchpad Drivers for Dual - screen Laptops under macOS](https://www.sqlsec.com/2021/12/screenpad.html) 

Since the article is rather long, I've extracted the key parts and put them in this tutorial. Interested friends can also go to the original article to read it. Now, let's directly start with the compilation tutorial of the VoodooI2C project.

Update: The VoodooI2C project has been constantly changing, so I don't have the energy to keep maintaining and updating this article. Although the project keeps changing, the basic ideas in this article are still worth referring to.

## Clone the Source Code

The modules on which the VoodooI2C project depends belong to different repositories. So, when using `git clone`, you need to use the `recursive` parameter to clone and download them all at once:

```bash
# The folder path for this storage is for reference only
$ pwd
/Users/bytedance/Desktop/XCode

$ cd ~/Desktop/XCode

$ git clone --recursive https://github.com/VoodooI2C/VoodooI2C.git
```

## Open the Project

Open the `VoodooI2C.xcworkspace` file using Xcode:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399676482692.webp) 

Opening the project won't be plain sailing. We'll see a lot of issues:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399684967484.jpeg) 

Actually, we don't need to worry about them here. Just keep the defaults. Don't manually click "Perform Changes".

## IOKit Missing

Click "Product" - "Build" to compile first, and you'll find an error reported: `'IOKit/IOLib.h' file not found`: 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399686079268.jpeg) 

This problem stuck me for a long time at first. Later, I found that in the "Build Settings" of the project, in the "Library Search Paths", I didn't have the `MacKernelSDK` folder:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399688648632.webp) 

That's why the IOKit library file can't be found. The solution is to manually supplement and complete the MacKernelSDK folder.

The open - source project address of MacKernelSDK maintained by the acidanthera guru is: https://github.com/acidanthera/MacKernelSDK

```bash
# Note the path of my current operation
$ pwd                                                           ✔
/Users/bytedance/Desktop/XCode/VoodooI2C

# Directly download this project
$ git clone https://github.com/acidanthera/MacKernelSDK.git
```

At this point, when you compile again, you'll find that the previous `'IOKit/IOLib.h' file not found` error has disappeared, and this problem has been solved.

## VoodooInputMultitouch Missing

After solving the above small problem, you'll find that there are new errors:

```ini
'VoodooInputMultitouch/VoodooInputTransducer.h' file not found
```

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399691969738.jpeg) 

Don't panic. This is a big problem because the corresponding files are missing in the project, and we also need to manually supplement them. Coincidentally, this project is also maintained by the acidanthera guru, and the address is: https://github.com/acidanthera/VoodooInput

Copy the `VoodooInputMultitouch` file to the **VoodooI2C/Multitouch\ Support/Native** directory:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1639969960300.jpeg) 

At this point, there should be no more compilation errors.

## cldoc or cpplint Missing

However, some people may always encounter various problems and may also encounter errors like `xargs: cldoc: No such file or directory` or `cpplint: command not found`. I've forgotten how to solve this. It seems that we can solve it by directly installing Python libraries:

```bash
sudo easy_install cldoc
sudo easy_install cpplint
```

It seems that since macOS 12.3, Python2 has been removed, and there is no easy_install command. We can use the brew command instead:

```bash
brew install cpplint
```

Then, there may still be the problem of the cldoc command missing. Besides installing the corresponding cldoc command, you can also directly remove the "Generate Documentation" part in the project:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16464793321944.webp)

I've forgotten more specific details, but this part isn't complicated. Since you've already reached the step of Xcode compilation, this problem shouldn't be too difficult for you.

## Compile the Project

So far, all the errors have been solved. Now, let's directly compile and generate kexts. First, click "build" to compile. It's very smooth:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399702435479.webp) 

Then click "Product" and then "Archive" to package the project:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399708239549.jpeg) 

Then a window will pop up. Select the latest version we packaged and select "Distribute Content".

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399708882219.jpeg) 

Later, just keep the default "Build Products":

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399709722736.webp) 

Finally, the compiled kexts files will be on the desktop:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399711135906.jpeg) 

## Pitfall Record

The Pluglns of the `VoodooI2C.kext` we compiled are incomplete, and there are two aliases inside:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16399712181607.jpeg) 

The solution can be to manually copy the kext outside the directory in, or directly use the officially compiled `VoodooI2C.kext`.

## Support?

In this noisy and impetuous era, how many people still insist on writing blogs and outputting original articles? Writing a blog always feels like generating electricity with love...

If you happen to be wealthy and feel that this article has helped you, you can consider making a donation to this article to maintain the high - cost server operation (domain name cost, server cost, CDN cost, etc.)