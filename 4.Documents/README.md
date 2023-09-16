
# BeagleBoneBlack

![Screenshot from 2023-09-10 11-50-32](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/4e2654ec-c2b2-463d-9735-2a0e1be3d1ea)

### GPMC  of the AM335X SOC used for 
-	Interfacing of external NAND flash
-	Interfacing of externel NOR flash
-	Interfacing of external SRAM

### EMIF of the AM335X SOC used for 
- Interfacing of Dynamic RAM (DDR)

### In BBB Rev.C EEPROM is connected to  
-	A single **4KB EEPROM** is provided on **I2C0**. **EEPROM** holds the board information that includes board name, serial number, and revision information

### There is no DSP co-processors in AM335x 

  

The AM335x is complex piece of hardware, but has limited internal RAM (128 kB)

Because of this limited amount of RAM, multiple bootloader stages are needed

These bootloader stages systematically unlock the full functionality of the device so that all complexities of the device are available to the kernel

## The AM335x has four distinct bootloader stages:
	1) ROM
	2) SPL (or Secondary Program Loader)
	3) U-BOOT
	4) Linux Kernel

	+--------+----------------+----------------+----------+
	| Boot   | Terminology #1 | Terminology #2 | Actual   |
	| stage  |                |                | program  |
	| number |                |                | name     |
	+--------+----------------+----------------+----------+
	| 1      |  Primary       |  -             | ROM code |
	|        |  Program       |                |          |
	|        |  Loader        |                |          |
	|        |                |                |          |
	| 2      |  Secondary     |  1st stage     | u-boot   |
	|        |  Program       |  bootloader    | SPL/ MLO |
	|        |  Loader (SPL)  |                |          |
	|        |                |                |          |
	| 3      |  -             |  2nd stage     | u-boot   |
	|        |                |  bootloader    |          |
	|        |                |                |          |
	| 4      |  -             |  -             | kernel   |
	|        |                |                |          |
	+--------+----------------+----------------+----------+


1st Stage Bootloader: ROM Code
--------------------------------

The first stage bootloader is flashed in ROM on the device by Texas Instruments.

The ROM code is the first block of code that is automatically run on device start-up or after power-on reset (POR)

The ROM bootloader code is hardcoded into the device and cannot be changed by the user.

The ROM code has two main functions:

- Configuration of the device and initialization of primary peripherals
	-- Stack setup
	-- Configure Watchdog Timer 1 (set to three minutes)
	-- PLL and System Clocks configuration

- Ready device for next bootloader
	-- Check boot sources for next bootloader (SPL)
	-- Moves next bootloader code into Internal RAM memory(128kb) to be run

The main purpose of the ROM code is to set up the device for the second stage bootloader.

## By default, the ROM code in the Sitara AM3359 will boot from 
	MMC1 interface first (the onboard eMMC)
	MMC0 (external uSD)
	UART0 
	USB0

## If the boot switch (S2) is held down during power-up, the ROM will boot from 
	SPI0 Interface first
	MMC0 (external uSD)
	USB0
	UART0

2nd Stage Bootloader: SPL
------------------------------

A fully featured version of U-Boot can be over 400KB, and the internal RAM on the AM335X is 128KB

Hence it is not possible to load this immediately

For this reason, a cut down version of U-Boot called U-Boot SPL (Secondary Program Loader) is loaded first,

once it has initialised the CPU, it chain loads a fully featured version of U-Boot (u-boot.img).

Name of SPL should be MLO

It should be located on active first partition of MMC, which must be formatted as FAT12/16/32


3rd Stage Bootloader - U-Boot
-------------------------------

U-BOOT allows for powerful command-based control over the kernel boot environment via a serial terminal

The user has control over a number of parameters such as boot arguments and the kernel boot command

In addition, U-boot environment variables can be configured. 

These environment variables are stored in the uEnv.txt file on your storage medium.

The built-in environment in u-boot loads a default am335x-boneblack.dts to pass to the kernel at boot.

In uEnv.txt you can explicitly specify a different DTS as well as the command line arguments to pass to the kernel

u-boot is also capable of obtaining network information via DHCP and loading it into environmental variables.

Finaly U-boot loads the kernel and a DTS into memory and boots the kernel with some command line arguments

The kernel initializes and mounts the root filesystem.

By default, the root filesystem is contained in the second partition (mmcblk0p2) of the microSD card, formatted for an ext3 file system.


Serial setup
=================

BeagleBone Black uses a serial debug port to communicate with the host machine. We will use minicom as a serial terminal client to communicate over the serial port. To set up minicom, perform the following steps:

1. Run this setup command as a privileged user:

    $  sudo minicom -s

2. Press E to set the baud rate. Use the A and B keys to navigate the baud rate values. A corresponds to next and B to previous. Keep pressing B till you get 115200 8N1. Then, press Enter to choose this setting and go back to the previous menu.

3. Next, we need to press F and G to change enablement statuses of hardware flow control and software flow control. Both need to be set to No.

4. Choose Save setup as dfl to avoid reconfiguring every time and choose Exit to go to minicom. Don't exit from it if you want to observe whether there is any activity on the serial port.



