<h1> Linux Device Driver Using BeagleBone Black </h1>

## 1. Introduction of Linux Source Tree

![Screenshot from 2023-09-10 21-42-34](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/cd76d6bf-083d-43a5-a1db-45c9fe8c5b2e)

![Screenshot from 2023-09-10 22-37-44](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/f01fe134-e0e7-4005-93eb-2cce3d44a1f5)
![Screenshot from 2023-09-10 21-47-17](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/40ae4626-8961-435f-ac96-ed6801492566)

## CPU Specific Arm Architecture Codes 

![Screenshot from 2023-09-10 21-43-50](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/cfaac292-a044-4fae-bf59-394c6523e5d0)
![Screenshot from 2023-09-10 21-44-42](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/2a706113-fb77-4cd3-bd74-391ef58ebce7)

![Screenshot from 2023-09-10 21-46-41](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/63b7d847-f38c-4208-aef3-d148afcb9233)

## Here from dtb file they match the String "dt_compact" to decide the specific Board in the board-generic.c file

![Screenshot from 2023-09-10 22-17-04](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/72ebd258-5e12-467c-bfd3-efe510b05493)

## Some common Device Driver in Sitara family by TI

![Screenshot from 2023-09-10 22-20-12](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/8e8cbb71-6342-45c1-adc6-27128ec94933)

## Some Top level Driver can in-build Module where its sub-driver can be Dynamic Loadable Module
![Untitled design](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/7d156644-2612-43ac-a260-98d8b1540491)

## 2.Linux Device Driver

- Device driver is a piece of code that configures and manages a device. 
- The device driver code knows, how to configure the device, sending data to the device, and it knows how to process requests which originate from the device. 
- When the device driver code is loaded into the operating system such as Linux, **it exposes interfaces to the user-space** so that the user application can communicate with the device. 
- Without the device driver, the OS/Application will not have a clear picture of how to deal with a device.
- There are three types of device drivers:
    + **Character device drivers**: character devices (RTC, keyboard, sensor,...)
    + **Block device drivers**: storage devices (mmc, eeproom, flash, harddisk,...)
    + **Network device drivers**: network devices (ethernet, wifi, bluetooth,...)

<p align="center"><img width="900" src="https://user-images.githubusercontent.com/32474027/94214748-5db8a700-ff15-11ea-8ee6-8e500a9f9f9a.PNG" \></p>

- **Kernel space**: This is a set of addresses where the kernel is hosted and where it runs. Kernel memory (or kernel space) is a memory range, owned by the kernel, protected by access flags, preventing any user apps from messing with the kernel (un)knowingly. On the other hand, the kernel can access the whole system memory, since it runs with the higher priority on the system. In kernel mode, the CPU can access the whole memory (both kernel space and user space).

- **User space**: This is a set of addresses (locations) where normal programs (such as `gedit` and so on) are restricted to run in. You may consider it a sandbox or a jail, so that a user program can't mess with memory or any other resource owned by another program. In user mode, the CPU can only access memory tagged with user space access rights. The only way for a user app to run in the kernel space is through system calls. Some of these are `read`, `write`, `open`, `close`, `mmap`, and so on. User space code runs with lower priority. **When a process performs a system call, a software interrupt is sent to the kernel, which turns on privileged mode so that the process can run in kernel space**. When the system call returns, the kernel turns off the privileged mode and the process is jailed again.


## 3. Source organization

- `arch/`: The Linux kernel is a fast growing project that supports more and more architectures. That being said, the kernel wants to be as generic as possible. Architecture-specific code is separated from the rest, and falls into this directory. This directory contains processor-specific subdirectories such as `alpha/`, `arm/`, `mips/`, `blackfin/`, and so on.
- `block/`: This directory contains code for block storage devices, actually the scheduling algorithm.
- `crypto/`: This directory contains the cryptographic API and the encryption algorithms code.
- `Documentation/`: This should be your favorite directory. It contains the descriptions of APIs used for different kernel frameworks and subsystems. You should look here prior to asking any questions on forums.
- `drivers/`: This is the heaviest directory, continuously growing as device drivers get merged. It contains every device driver organized in various subdirectories.
- `fs/`: This directory contains the implementation of different filesystems that the kernel actually supports, such as *NTFS*, *FAT*, *ETX{2,3,4}*, *sysfs*, *procfs*, *NFS*, and so on.
- `include/`: This contains kernel header files.
- `init/`: This directory contains the initialization and start up code.
- `ipc/`: This contains implementation of the **Inter-Process Communication (IPC)** mechanisms, such as *message queues*, *semaphores*, and *hared memory*...
- `kernel/`: This directory contains architecture-independent portions of the base kernel.
- `lib/`: Library routines and some helper functions live here. They are generic **kernel object (kobject)** handlers, **Cyclic Redundancy Code (CRC)** computation functions, and so on.
- `mm/`: This contains memory management code.
- `net/`: This contains networking (whatever network type it is) protocols code.
- `scripts/`: This contains scripts and tools used during kernel development. There are other useful tools here.
- `security/`: This directory contains the security framework code.
- `sound/`: Audio subsystems code is here.
- **`usr/`: This currently contains the initramfs implementation.**

## 4. Kernel configuration
- In most cases, there will be no need to start a configuration from scratch. There are default and useful configuration files available in each `arch/` directory, which you can use as a starting point:
```shell
ls arch/<you_arch>/configs/ 
```
- For ARM-based CPUs, these configs files are located in `arch/arm/configs/`, and for an BeagleBone Black processor, the default file config is `arch/arm/configs/bb.org_defconfig`. Similarly, for x86 processors we find the files in `arch/x86/configs/`, with only two default configuration files, `i386_defconfig` and `x86_64_defconfig`, for 32-bit and 64-bit versions respectively.
- For an x86 system:
```shell
make x86_64_defconfig
make uImage -j4
make modules
make INSTALL_MOD_PATH=</where/to/install> modules_install
```
- For BBB board:
```shell
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- bb.org_defconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- uImage dtbs LOADADDR=0x80008000 -j4
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- modules -j4
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- INSTALL_MOD_PATH=</where/to/install> modules_install
```
















