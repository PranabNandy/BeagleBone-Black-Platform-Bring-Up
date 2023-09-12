<h1> U-boot </h1>

## U-boot Commands
![Screenshot from 2023-09-10 23-14-29](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/e2ac0887-1b98-46b8-a7df-0a4e10ec7ea8)

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

-  ![Screenshot from 2023-09-10 18-22-17](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/7d45376e-70b6-41f9-a434-688865bc6bfd)


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
--  ![Screenshot from 2023-09-10 23-25-05](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/a4cb3b0a-1453-4b95-9f96-658b95d8d159)
--  ![Screenshot from 2023-09-10 23-25-29](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/1fb1b3eb-c878-44b7-b56b-d8c5cd6cece0)




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




## Booting BBB over Serial Port (UART0 peripheral Port)

#### Peripheral Booting


The ROM Code can boot from three different peripheral interfaces:
-  EMAC: 1000/100/10 Mbps Ethernet, using standard TCP/IP network boot protocols BOOTP and TFTP
-  USB: Full speed, client mode
-  UART: 115.2Kbps, 8 bits, no parity, 1 stop bit, no flow control

The purpose of booting from a peripheral interface is to download a boot image from an external host (typically a PC). This booting method is mostly used for programming flash memories connected to the device (e.g. in the case of initial flashing, firmware update or servicing).

- The ROM code will ping the host **10 times** in **3s to start** x-modem transfer. If host does not respond, **UART boot will timeout**.

- **115200** is the baudrate that **ROM boot loader expects SPL** from Host Machine.

- **ROM bootloader** does not support booting over **UART1 Peripheral** 

- ![Screenshot from 2023-09-10 21-17-38](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/6fd7d4b5-b698-41c1-9d7e-48fe6e0bf487)
- ![Screenshot from 2023-09-10 19-10-55](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/330bd88a-84f4-4437-b82b-e573c6420fd9)

- ![Screenshot from 2023-09-10 23-15-24](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/ec511a55-8090-40ff-be4a-b7cf376bee4f)


# Booting BeagleBone-Black over TFTP Protocol

**TFTP stands for Trivial File Transfer Protocol which transfer files between TFTP server and TFTP client.**

Step 1:  First on your Ubuntu host run the below command using your terminal program.

This command installs the tftpd , xinetd(eXtended InterNET Daemon) . tftpd is a server for the Trivial File Transfer Protocol.

```$ sudo apt-get install xinetd tftp tftpd```

Step 2 : Create/Open the file “tftp” in the below directory

```$ sudo vi /etc/xinetd.d/tftp    and put the below entry in to this file  and save it```

```
service tftp
{
protocol = udp
port = 69
socket_type = dgram
wait = yes
user = nobody
server = /usr/sbin/in.tftpd
server_args = /var/lib/tftpboot -s
disable = no
}
```

Step3 : Create a folder /var/lib/tftpboot and execute below commands

```
$sudo mkdir /var/lib/tftpboot

$sudo chmod -R 777 /var/lib/tftpboot

$sudo chown -R nobody /var/lib/tftpboot

Step 4: Restart the xinetd service. Now the xinetd daemon is running.

$ sudo /etc/init.d/xinetd restart



$ sudo ifconfig enp1s0 192.168.27.1
```

----------

```
# setenv serverip 192.168.27.1

# setenv ipaddr 192.168.27.2

# tftpboot 0x82000000 uImage

# tftpboot 0x88000000 am335x-boneblack.dtb

# tftpboot 0x88080000 initramfs

# setenv bootargs console=ttyO0,115200 root=/dev/ram0 rw initrd=0x88080000

# bootm 0x82000000 0x88000000 0x88080000


# tftp -r helloworld -g 192.168.27.1

# ifconfig eth0 192.168.27.1

```

**uEnv.txt** for this case
```
console=ttyO0,115200n8
ipaddr=192.168.7.2
serverip=192.168.7.1
absolutepath=/var/lib/tftpboot/
rootpath=/srv/nfs/bbb,nolock,wsize=1024,rsize=1024 rootwait rootdelay=5
loadtftp=echo Booting from network ...;tftpboot ${loadaddr} ${absolutepath}uImage; tftpboot ${fdtaddr} ${absolutepath}am335x-boneblack.dtb
netargs=setenv bootargs console=${console} root=/dev/nfs rw nfsroot=${serverip}:${rootpath} 
uenvcmd=setenv autoload no; run loadtftp; run netargs; bootm ${loadaddr} - ${fdtaddr}
```
