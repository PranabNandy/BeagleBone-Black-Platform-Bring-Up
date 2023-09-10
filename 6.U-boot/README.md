<h1> U-boot </h1>

## U-boot Commands

### After you load MLO followed u-boot.img, a terminal will appear 

```
# printenv soc

# setenv serverip 192.168.27.1

# printenv serverip

# setenv my_mmc_list 'mnc list'

# run findfdt

# mnc rescan

# mnc list

# help

# help load

------- Multi commands -----------------------
mmcboot=echo Booting from microSD ..; setenv autoload no ; run bootsettings ; 
```


```
# load mnc 1:2 0x82000000 /BOOT/uImage

# bootm 0x82000000

---> Linux bootstrap loader looks for the dtb file

# load mnc 1:2 0x88000000 /BOOT/am335x-boneblack.dtb

# bootm 0x82000000 - 0x88000000

# setenv bootargs console=tty00,115200 root=/dev/mncblk0p2;

# loady 
---- ctrl + A ==> S  ==> double space (select directory) 
==> single space bar(select file uEnv.txt)

# env import -t 0x80200000 29

```
#### Final uEnv.txt 

```
bootsettings=setenv bootargs console=ttyO0,115200n8 root=/dev/mmcblk0p2 rw rootfstype=ext4 rootwait earlyprintk mem=512M
mmcboot=echo Booting from microSD ...; setenv autoload no ; load mmc 0:1 ${loadaddr} uImage ; load mmc 0:1 ${fdtaddr} am335x-boneblack.dtb ; run bootsettings ; bootm ${loadaddr} - ${fdtaddr}
uenvcmd=run mmcboot

```

## initramfs

### So, why do we need initramfs ?

Let’s understand this with an example. 

- Let’s say you have a product and your product has USB interfaces, mass storage devices like SD card and let’s say you also have networking peripherals like Ethernet and also display. 

- Now, to operate this wide range of peripherals, all the device drivers must be in place right?

- That means all the drivers must be loaded in to the kernel space.

- And along with the drivers, some peripherals may require the **firmware binaries to operate**. 

- One idea is make sure that all those drivers are “built in” in to the kernel, that’s not a great idea because that makes your Linux kernel specific to your product and it will drastically increase the Linux kernel image size. 

- Another good way is, you come up with the minimal file system, where you store all your drivers and firmware, and **load that FS in to the RAM** and ask the **Linux to mount that file system during boot**( Thanks to kernel boot arguments , you can use the kernel boot arguments to indicate kernel that your FS resides in RAM )

- When the kernel mounts that file system from RAM, it loads all the required drivers for your product and all the peripherals of your product are ready to operate, because the drivers are in place. 

- after That you can even get rid of this RAM based file system and use (switch to) some other **advanced file system** which resides on your other memory devices like **eMMC/SD card** or even you can **mount from the network**. 

- So, basically initramfs embedded into the kernel and loaded at an early stage of the boot process, where it gives all the minimal requirements to boot the Linux kernel successfully on the board just from RAM without worrying about other peripherals. And what you should store in initramfs is left to your product requirements, you may store all the important drivers and firmware, you may keep your product specific scripts, early graphic display logos, etc. 


## How to keep initramfs in to RAM?

 There are 2 ways, 

 1) You can make initramfs “built in” in to the Linux Kernel during compilation , so when the Linux starts booting , it will place the initramfs in the RAM and mounts as the initial root file system and continues.

 2) You can load the initramfs from some other sources in to the RAM of your board and tell the Linux Kernel about it (that is , at  what RAM address initramfs is present ) via the kernel boot arguments. 



## How to generate the innitramfs ?

Follow these steps to generate the initramfs

Reproduce these steps at your desk, because we are going to use the generated initramfs  in our course as we move along. 

- Step 1: Download and extract the root file system attached with this lecture. This root file system I took from TI software SDK

![2017-08-21_09-40-17-9f3d6e948e6486f6f2cba9457ae2465b](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/38a3e32b-f806-4f03-b5e8-19677d240dc6)



- Step 2: Get in to the extracted folder and run the below 2 commands from the “terminal”

```
$ find . | cpio -H newc -o > ../initramfs.cpio

$ cat ../initramfs.cpio | gzip > ../initramfs.gz

In the first command we are generating a cpio archive and then gz archive.

You will end up with the file “initramfs.gz” which is about 3MB.
```

![2017-08-21_09-40-42-8182a5e3a78b17466f9cdea1b26f0e5d](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/04d3c225-b1e2-49d6-bb81-8c3120a4f5ec)





- STEP 3 : install mkImage command

```

For that run “apt-get install u-boot-tools” on your terminal software, this will install all the u-boot related tools along with mkImage tool.

$ apt-get install u-boot-tools

STEP 4: Make our initramfs , uboot friendly by attaching the uboot header with load  address and other info .

Run the below command.

$ mkimage -A arm -O Linux -T ramdisk -C none -a 0x80800000 -n "Root Filesystem" -d ../initramfs.gz  ../initramfs
```
![2017-08-21_09-41-34-b2c1e2f6ae0d61b48433a57e085f1d7b](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/df239e5e-80cd-42a4-be4f-7357a18ea494)


Great now you will end up with a file “initramfs” which also includes the uboot header and this file we will be used as ram based file system whenever required. 










