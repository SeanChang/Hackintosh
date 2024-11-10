Hackintosh often has problems with NVME hard drives. For example, common ones include Samsung PM961/PM981/PM981a/PM991 series, Micron 2200V MTFDHBA512TCK, Micron 2200S, etc. For details of hard drives with no solution, you can refer to the tutorial: [Hardware Limitation - Hard Drive Support](/Basic/Drivers/)

## PXPX Method

You need to first find the specific hard drive path under Windows. It's different for different computers. The actual path depends on your actual situation:

Then replace the content `_SB_.PCI0.RP21` below with the actual path you see, and then save the file as **SSDT - RP.PXSX - disbale.aml**. OC can load this SSDT:

```c
DefinitionBlock ("", "SSDT", 2, "OCLT", "noRPxx", 0x00000000)
{
    External (_SB_.PCI0.RP21, DeviceObj)

    Scope (_SB.PCI0.RP21)
    {
        OperationRegion (DE01, PCI_Config, 0x50, One)
        Field (DE01, AnyAcc, NoLock, Preserve)
        {
               ,   4, 
            DDDD,   1
        }

        Method (_STA, 0, Serialized)  // _STA: Status
        {
            If (_OSI ("Darwin"))
            {
                Return (Zero)
            }
        }
    }

    Scope (\)
    {
        If (_OSI ("Darwin"))
        {
            \_SB.PCI0.RP21.DDDD = One
        }
    }
}
```

## Method of Disabling the Discrete Graphics Card

I named this myself actually, because disabling the discrete graphics card is also in this way. Some NVME hard drive controllers can also use this method to be disabled.

You need to first find the specific hard drive path under Windows. It's different for different computers. The actual path depends on your actual situation:

![](https://seanchang.github.io/picx-images-hosting/20241109/xuanyuan.me-16517192033914.webp) 

According to the above picture, the path of our NVME hard drive is:

```
_SB_.PCI0.GPP0
```

Then replace the content `_SB_.PCI0.GPP0` below with the actual path you see:

```c
DefinitionBlock ("", "SSDT", 2, "DRTNIA", "spoof", 0x00000000)
{
    External (_SB_.PCI0.GPP0, DeviceObj)

    Method (_SB_.PCI0.GPP0._DSM, 4, NotSerialized)  // _DSM: Device - Specific Method
    {
        If ((!Arg2 || (_OSI ("Darwin") == Zero)))
        {
            Return (Buffer (One)
            {
                 0x03                                             //.
            })
        }

        Return (Package (0x0A)
        {
            "name", 
            Buffer (0x09)
            {
                "#display"
            }, 

            "IOName", 
            "#display", 
            "class - code", 
            Buffer (0x04)
            {
                 0xFF, 0xFF, 0xFF, 0xFF                           //....
            }, 

            "vendor - id", 
            Buffer (0x04)
            {
                 0xFF, 0xFF, 0x00, 0x00                           //....
            }, 

            "device - id", 
            Buffer (0x04)
            {
                 0xFF, 0xFF, 0x00, 0x00                           //....
            }
        })
    }
}
```