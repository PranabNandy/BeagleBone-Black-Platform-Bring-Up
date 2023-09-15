# Embedded System Development using BBB 
### BBB is a SBC (Single Board Computer) - it contains an AM335x which is a SoC (System on Chip). Am335x has a Cortex-A8 (A 32 bit MPU) where we run Linux & other HLOSes.
#### Beagle Bone Black Initial Boot up [Video](https://www.youtube.com/watch?v=R6c0kC25NRo&ab_channel=PranabNandy)



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
2. [Introduction to the BeagleBone Black Device Tree](https://learn.adafruit.com/introduction-to-the-beaglebone-black-device-tree/device-tree-overlays)
4. [The Beaglebone Black and Device Tree Overlays](http://www.ofitselfso.com/BeagleNotes/Beaglebone_Black_And_Device_Tree_Overlays.php)
3. [Sharing internet using Network-Over-USB in BeagleBone Black](https://elementztechblog.wordpress.com/2014/12/22/sharing-internet-using-network-over-usb-in-beaglebone-black/)

#### Why use BeagleBone Black​ ? 
	
-	Because it's an Open Source Platform (i.e open hardware board), taht makes a big difference if you need to redesign a complete board with only your needed functionnality. The number of GPIO pins available makes it amazing for extending the boards capability. 
-	For interacting with the physical world, the BB has 7 analog input channels, while the RPi has none. The BBB has a Programmable Realtime Unit (PRU) Subsystem that allows for much faster GPIO usage.
-	You can also install Android relatively easy onto here so if that’s a deal breaker then Beaglebone is for you. However, I did find Android a little slow on the Beaglebone. I would personally pick the Beaglebone Black if I were looking into more device or circuitry based projects. 

#### We will focus on upstream components:

	1. U-Boot: this is the most often used bootloader in embedded systems and it’s recommended for BBB. 
	We will use upstream U-Boot, as it supports BBB.
	2. Linux Kernel: it’s a main part of Linux OS. We will use upstream kernel, as it supports BBB.
	3. BusyBox rootfs: it’s minimal, so it’s the best choice for understanding the crucial OS parts. 
	It can be useful in real work for testing, debugging and isolating the bugs, due to its minimalistic 
	nature. It also often can be found in networking devices (routers, etc), and is used as initramfs 
	in desktop GNU/Linux distributions.


-	Beagle Black comes with a 1GHz AM3359 ARM Cortex A8 processor which uses ARMv7 instruction set
-	Raspberry Pi 3 has a 1.2GHz 64-bit quad-core ARMv8 CPU. 

#### 4 elements of BBB board End-to-End Bring Up with Upstream Components
-	Preparing the Tools

-	Obtaining and Building the Software

-	Flashing

-	Booting











