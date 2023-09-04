# BeagleBone-Black-Platform-Bring-Up
#### Beagle Bone Black Initial Boot up [Video](https://www.youtube.com/watch?v=R6c0kC25NRo&ab_channel=PranabNandy)

	Beagle Black comes with a 1GHz AM3359 ARM Cortex A8 processor which uses ARMv7 instruction set
	Raspberry Pi 3 has a 1.2GHz 64-bit quad-core ARMv8 CPU. 


<p align="center"> <img width="500" src="https://elementztechblog.files.wordpress.com/2014/11/beaglebone_black.jpg" /> </p>


## Objective of the Project
- Understanding various sub systems of AM335x (1GHz ARM® Cortex-A8) SOC such as GPIOs, I2C, SPI, MMC, etc...
- Understanding ROM–Uboot-Kernel boot process on Linux-ARM systems and testing.
- U-Boot, MLO, SPL explanation and testing.
- Linux device tree
- Understanding platform devices and how it works.
- eMMC OS updating and booting new Debian OS.
- Interfacing external LEDs, temperature sensor, Acceleration/Gyroscope such as MPU 6050 or ADXL345 and code development.
- Using HTTP, the sensors can push numeric or alphanumeric data to the server, where it can be processed and visualized.
- Beaglebone networking
- Serial log (UART) used minicom tool

## Prepare

| Hardware              | Sortware      | Reference                                                                                |
|-----------------------|---------------|------------------------------------------------------------------------------------------|
| Beaglebone Black Rev.C| Ubuntu 18.04  | Exploring BeagleBone Tools and Techniques for Building with Embeddded Linux ®Derek Molloy|
| MPU6050               | VSC or Vim    | [AM335x ARM® Cortex™-A8 Microprocessors (MPUs) Technical Reference Manual](https://e2e.ti.com/cfs-file/__key/communityserver-discussions-components-files/790/AM335x_5F00_techincal_5F00_reference_5F00_manual.pdf)|
| ADXL345               | Cross compile | [AM335x Datasheet](https://www.ti.com/lit/ds/sprs717l/sprs717l.pdf?ts=1598362140689&ref_url=https%253A%252F%252Fwww.google.com%252F)|
| Button                | BusyBox       | [Beagleboard Cape Expansion Headers](https://elinux.org/Beagleboard:Cape_Expansion_Headers)|  
| Leds                  | Buildroot     | [MPU-6000 and MPU-6050 Register Map and Descriptions Revision 4.2](https://invensense.tdk.com/wp-content/uploads/2015/02/MPU-6000-Register-Map1.pdf)|
| 74HC595               | Minicom       | [MPU-6000/MPU-6050 Product Specification](https://invensense.tdk.com/wp-content/uploads/2015/02/MPU-6000-Datasheet1.pdf)|
| TMP36                 | GParted       | [ADXL345 Datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/ADXL345.pdf)|
| FT232RL USB Serial    |               | [TMP36 Datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/TMP35_36_37.pdf)|
| SD card (>4GB)        |               | [74HC595 Datasheet](https://www.onsemi.com/pub/Collateral/MC74HC595-D.PDF)|
| RTC-DS3231            |               | [RTC-DS3231](https://datasheets.maximintegrated.com/en/ds/DS3231.pdf) 
| GPS-Neo6M             |

## How to connecting BBB to PC using external USB TO UART converter
![Screenshot from 2020-08-25 22-18-12](https://user-images.githubusercontent.com/32474027/91179255-3c4f7a00-e721-11ea-8006-a49083c3fb5e.png)

## Instruction list 
1. [CGI using C++ on the BeagleBone](http://derekmolloy.ie/beaglebone-cgicc/)
2. [Introduction to the BeagleBone Black Device Tree](https://learn.adafruit.com/introduction-to-the-beaglebone-black-device-tree/device-tree-overlays)\
4. [The Beaglebone Black and Device Tree Overlays](http://www.ofitselfso.com/BeagleNotes/Beaglebone_Black_And_Device_Tree_Overlays.php)
3. [Sharing internet using Network-Over-USB in BeagleBone Black](https://elementztechblog.wordpress.com/2014/12/22/sharing-internet-using-network-over-usb-in-beaglebone-black/)

#### Why use BeagleBone Black​ ? 
	
Because it's an open hardware board, it’s very convenient to use it for learning. Even electronics
bits could be studied on it. Also, it has a good design (as opposed to e.g. Raspberry Pi), which
resembles professional embedded devices design. This board is very flexible, so we can use it to learn C, Linux, Embedded, Kernel development, testing, etc. 

#### We will focus on upstream components:

	1. U-Boot: this is the most often used bootloader in embedded systems and it’s recommended for BBB. 
	We will use upstream U-Boot, as it supports BBB.
	2. Linux Kernel: it’s a main part of Linux OS. We will use upstream kernel, as it supports BBB.
	3. BusyBox rootfs: it’s minimal, so it’s the best choice for understanding the crucial OS parts. 
	It can be useful in real work for testing, debugging and isolating the bugs, due to its minimalistic 
	nature. It also often can be found in networking devices (routers, etc), and is used as initramfs 
	in desktop GNU/Linux distributions.

## The only thing we can’t use this board for is Android.

#### 4 elements of BBB board End-to-End Bring Up with Upstream Components
-	Preparing the Tools

-	Obtaining and Building the Software

-	Flashing

-	Booting




#### Copying images to the card
    $ sudo mount /dev/sdb1 /media/$USER/BOOT
    $ sudo mount /dev/sdb2 /media/$USER/ROOT
    
    1. Copy the u-boot MLO and u-boot bootloader images into the FAT32 partition:
    
    $ sudo cp MLO /media/$USER/BOOT
    $ sudo cp u-boot.img /media/$USER/BOOT

    2. Copy the kernel image into the boot partition:

    $ sudo cp zImage /media/$USER/BOOT 

    3. Copy the .dtb file, am335x-boneblack.dtb, into the boot partition. This step is required only in the case of core-image-minimal. It is not          required in our case, as we created core-image-sato, which already has this file placed at the desired location in rootfs:

    $ sudo cp am335x-boneblack.dtb /media/$USER/BOOT 

    4. As a root user, uncompress core-image-sato-beaglebone.tar.bz2 to the ext4 partition:

    $ sudo tar -xf core-image-minimal-beaglebone-yocto.tar.bz2 -C /media/$USER/ROOT/
    5. Unmount both partitions:

    $ sudo umount /dev/mmcblk0p1
    $ sudo umount /dev/mmcblk0p2


Gparted
-------------

#### First partition
type: FAT32
size: around 30MB
label: BOOT
flags: boot
 
#### Second partition
type: ext4
size: around 200MB, or rest of SD-card
label: ROOT

Serial setup
=================

BeagleBone Black uses a serial debug port to communicate with the host machine. We will use minicom as a serial terminal client to communicate over the serial port. To set up minicom, perform the following steps:

1. Run this setup command as a privileged user:

    $  sudo minicom -s

2. Press E to set the baud rate. Use the A and B keys to navigate the baud rate values. A corresponds to next and B to previous. Keep pressing B till you get 115200 8N1. Then, press Enter to choose this setting and go back to the previous menu.

3. Next, we need to press F and G to change enablement statuses of hardware flow control and software flow control. Both need to be set to No.

4. Choose Save setup as dfl to avoid reconfiguring every time and choose Exit to go to minicom. Don't exit from it if you want to observe whether there is any activity on the serial port.


Booting BeagleBone
====================

Now that we have everything set up, we are ready to boot.

We can just insert this card, and our board should boot from it. 

There might be only one issue if you have the eMMC (embedded MultiMediaCard) boot selected by default. 

You will have to disable it by booting up the board from the images you already have and renaming the MLO file from the eMMC partition

    $ mmc dev 1
    
    $ mmc erase 0 512

The first command will select the eMMC card, and the second one will do the erasing so that BeagleBone doesn't try to boot from eMMC.

To stop at the u-boot prompt, simply press Enter after powering up the board before timeout: Then U-boot promt will up.

#### Booting from mmc ...
    Booting kernel from Legacy Image at 82000000 ...
    Image Name:   Linux-3.14.0-yocto-standard
    Image Type:   ARM Linux Kernel Image (uncompressed)
    Data Size:    4985768 Bytes = 4.8 MiB
    Load Address: 80008000
    Entry Point:  80008000
    Verifying Checksum ... OK
    Flattened Device Tree blob at 88000000
    Booting using the fdt blob at 0x88000000
    Loading Kernel Image ... OK
    Loading Device Tree to 8fff5000, end 8ffff207 ... OK
    Starting kernel …

    Finally, we will land in our BeagleBone prompt:

    Poky (Yocto Project Reference Distro) 1.6.1 beaglebone /dev/ttyO0
    beaglebone login:

    Enter root as user, and you are in as the root user:

    root@beaglebone:~# 


