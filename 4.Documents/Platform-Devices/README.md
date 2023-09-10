
# BeagleBoneBlack

#### All those devices who can not announce themeselves(i.e their presence) to OS is known as platform devices.

-	USB is not Platform devices

#### let's say, if i connect DDR3 RAM to my laptop's pci connector ? Can i say DDR3 memory device is a platform device ?

-	DDR connected to PCI bus which is self discoverable

![Screenshot from 2023-09-10 16-03-05](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/268b252d-cd21-4cb9-829b-b8dea21c9262)

## To add all platform devices 

![Screenshot from 2023-09-10 16-06-27](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/885e0990-6863-4e44-898e-d9397196311f)


## Problem with my-board-config-file.c

![Screenshot from 2023-09-10 16-06-23](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/2c8e28df-a818-45e3-8396-b6134195e301)

## Second Problem with multiple boards

![Screenshot from 2023-09-10 16-11-15](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/647ecab9-85ea-408f-8f5f-7d36b0180d79)

## Solution to this problem is DTS (Device Tree Source) file 

![Screenshot from 2023-09-10 16-11-36](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/1a63dd39-cddf-4329-8128-ee8d6f073661)

## Compile this DTS file using DTC to generate DTB (Device Tree blob) file

![Screenshot from 2023-09-10 16-10-03](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/ed29c769-f465-407f-b641-3e07df1ebd29)


