## Recovery

### EB

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517246284025.webp) 

### Initialization

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517249478895.webp) 

### Loading ACPI

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517251206663.webp)  

Then load VirtualSMC and start running the CPU - related part of ACPI:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517252553068.webp) 

### GPU Sensor and NVME

AMD GPU sensor and NVME related:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517254169254.webp) 

Then it can be seen that the NVME controller is successfully loaded:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517255657636.webp)  

### NVRAM and Security Policy

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517261783169.webp) 

### Network and Hard Disk - related

The Ethernet card driver is loaded, and the disk utility is initialized, involving the hard disk part:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517266097712.webp) 

### XCMP Frequency Conversion, Boot Option Loading

XCMP is related to XNU power management. In addition, the boot - args boot option is also loaded:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517269976899.webp) 

### Wireless Network Card - related

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517272662528.webp) 

### Loading the Settings of the Base System

Including: system language, files of the base system, etc.

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517274497167.webp)

### Base System Loading

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1651727676426.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517279549134.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517280083985.webp) 

### USB Controller

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517281192231.webp) 

### Base System Loading

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517282979302.webp)  

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517283512558.webp) 

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517284213216.webp) 

### Completion of Base Loading

Generally, the integrated graphics are loaded here, and then USB and these also start to be completed:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517285654054.webp)   

## Normal Boot into the System

The normal loading process is similar to the above Recovery process, and there are not too many differences. I won't elaborate on it here. Only the differences will be listed.

### Base System Loading

The difference is that after the base system is loaded, the normal system will detect the USB controller here:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-1651728936671.webp) 

### Completion of Base Loading

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517290851414.webp)  

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517291508218.webp) 

### Sign of Success

When the following marked situation appears, it is mostly going to be successful:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517292444454.webp)