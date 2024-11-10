The Hackintosh USB flash drive has been created. Now, I'll briefly explain the current structure division of the USB flash drive and what its functions are. Without further ado, let's look at the pictures directly:

Under macOS, you can use Hackintool to directly and visually view the structural information of the disk:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16318012614605.webp) 

If you are using Windows, you can also view it by using the classic disk tool DiskGenius. Now, I'll show you what's in these three partitions one by one.

## Mount EFI
In most cases, the EFI partition is hidden by default. Under the Hackintool, we can right - click and select "Mount". After entering the password, the EFI partition can be mounted:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16318013455967.webp) 

After mounting the EFI partition, all three partitions on the desktop are displayed:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16318017737999.webp)     

## EFI
The default contents of the files in the EFI partition are as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16318018553429.webp) 

Senior Xiaobing also put two apps, and we can choose to keep them selectively.

The default EFI folder is for Clover, so later we need to delete the entire EFI folder and then copy the EFI of OC in. 

## WEPE
The default contents of the files in the WEPE partition are as follows:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16318019728766.webp) 

We usually don't need to worry about this partition. Although there is also an EFI folder here, it is used to boot Windows PE. Just be aware of it. 

## Install macOS XXX
The official Apple system installation package is in the Install macOS XXX partition. Actually, it is in the format ending with.app. Basically, all Apple applications are in this format. In fact, after we successfully install Hackintosh later, we can also manually download this image in the AppStore:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16318020277484.webp) 



So far, the boot disk has been initially created. Next, we need to configure an EFI driver configuration file suitable for our computer model. After configuring the EFI, just put it directly into the EFI partition of the USB flash drive.