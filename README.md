# BeagleBone-Black-Platform-Bring-Up

## Copying images to the card
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

## First partition
type: FAT32
size: around 30MB
label: BOOT
flags: boot
 
## Second partition
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

## Booting from mmc ...
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


