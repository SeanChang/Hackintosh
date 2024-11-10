## Finished Product Effect

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517302956257.webp) 

The script of [HiDPi](/6 - Practical Postures/6 - 5.html#reloaded) can also be used to customize the monitor icon. However, giving someone a fish is not as good as teaching them how to fish. Without further ado, here comes the tutorial.

## View Monitor Information

Use Hackintool to view monitor information:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517303732971.webp) 

Take the above DP monitor as an example and extract the key information (which will be used later):

- Vendor ID = **216D**
- Product ID = **2800**

## Create Folders and Icon Files

Go to the `/Library/Displays/Contents/Resources/Overrides` folder. There are only two steps:

1. Create the `DisplayVendorID - {Vendor ID}` folder.
2. Name your icon as `DisplayProductID - {Product ID}.icns`.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517305884893.webp) 

Just restart after the replacement.