# OC Editor

Since the integrated graphics configuration file of OpenCore ends with **.plist**, a specialized editor is required for editing. The advantages and disadvantages of common OC editors are as follows:

| Editor Name                                                   | Advantages           | Disadvantages                        |
| ------------------------------------------------------------ | -------------------- | ------------------------------------- |
| [ProperTree](https://github.com/corpnewt/ProperTree)         | Powerful, professional, cross - platform | High difficulty in getting started    |
| [OpenCore Configurator](https://mackie100projects.altervista.org/download - opencore - configurator/) | Powerful and simple  | Only available for Mac version        |
| [OCAuxiliaryTools](https://github.com/ic005k/OCAuxiliaryTools) | Cross - platform and simple | ~~The user experience needs improvement~~ It has become more and more powerful currently |

My suggestions are as follows:

1. If you have a Mac system available, OpenCore Configurator is the first choice.
2. For novice Windows users, OCAuxiliaryTools is recommended.

The following is a brief introduction to the installation and usage methods of these editors respectively.

!!! warning "Reminder: Different versions of OC configurations require the use of corresponding versions of OC editors, otherwise mysterious problems may occur."

## ProperTree

The official project address is: [https://github.com/corpnewt/ProperTree](https://github.com/corpnewt/ProperTree)

### Usage on the macOS Platform

Due to the fact that the macOS comes with a Python environment, double - click on **ProperTree.command** to open the program:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16319577307684.webp) 

Some main operations can be completed through the macOS menu.

### Usage on the Windows Platform

Windows 10 does not come with a Python environment by default, so you first need to install the Python environment. First, download Python3. Here, a more stable version of Python 3.8 is recommended. The official download address is: [https://www.python.org/ftp/python/3.8.10/python - 3.8.10 - amd64.exe](https://www.python.org/ftp/python/3.8.10/python - 3.8.10 - amd64.exe)

When installing, remember to check: **Add Python 3.8 to PATH**:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16319580501860.webp) 

This will automatically configure the environment variables, and we won't need to configure them manually later. Using the default path or a custom path is fine. I'm a bit lazy and used the default 「Install Now」. If you want to use a custom path, it's best that this path doesn't contain Chinese characters, otherwise some mysterious problems may occur.

To verify whether the installation is successful, open the command line and enter:

```bash
python - V
```

You can see the Python version information indicating success:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16319582235848.webp) 

After it is indeed installed successfully, double - click on the **ProperTree.bat** file to open the program:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16319585375960.webp) 

Similarly, some basic operations can be completed through File in the menu.

## OpenCore Configurator

The download address of the latest version of OpenCore Configurator is:

[https://mackie100projects.altervista.org/download - opencore - configurator/](https://mackie100projects.altervista.org/download - opencore - configurator/)

Find the configuration file you want to edit, right - click, select 「Open with」, and then choose 「OpenCore Configurator」:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16319587595893.webp) 

The main interface after opening is as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16319588277424.webp) 

## OCAuxiliaryTools

OCAuxiliaryTools is recommended to be used in Windows to replace ProperTree, because there is a better OpenCore Configurator with a better user experience available under macOS.

The official project address is: [https://github.com/ic005k/OCAuxiliaryTools](https://github.com/ic005k/OCAuxiliaryTools)

The Chinese instructions are: [https://github.com/ic005k/OCAuxiliaryTools/blob/master/READMe - cn.md](https://github.com/ic005k/OCAuxiliaryTools/blob/master/READMe - cn.md)

Download the corresponding Windows version. The main interface is as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16319601601628.webp) 

So far, the introduction of OC editors is completed. For detailed usage methods, you can refer to the corresponding B - station tutorial video of this tutorial. Next, it's time to prepare to configure the EFI file.