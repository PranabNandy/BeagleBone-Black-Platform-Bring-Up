<h1> Flash eMNC1 </h1>

#### Check To check debian latest Image 
-  `$ lsb_release -da`

Download latest Debain image from here https://www.beagleboard.org/distros

I used this one https://www.beagleboard.org/distros/am335x-debian-11-7-2023-08-05-4gb-emmc-iot-flasher
As it was latest as of now.

![Screenshot from 2023-09-08 01-27-46](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/1df57ca7-a487-48d6-b573-800cdebac87c)

Install Balena Etcher from https://etcher.balena.io/

I downloaded in Windows 10 and flash the rootfs(am335x-debian-11-7-2023-08-05-4gb-emmc-iot-flasher) in microSD card.

![Screenshot from 2023-09-08 01-32-02](https://github.com/PranabNandy/BeagleBone-Black-Platform-Bring-Up/assets/80820274/0efa99c5-0bd5-41be-81f3-5ef44567602b)

### Press the boot button and power the cable

----------------------------------------------------------------------------------------------------------------------
# Enable Internet over USB
```
debian@BeagleBone:~$ cd /opt/script/boot/
-----> Here the usb0 and usb1 over ethernet scripts are present

debian@BeagleBone:~$ sudo vi /etc/resolv.conf

nameserver 8.8.8.8
nameserver 8.8.4.4

debian@BeagleBone:~$ sudo vi /etc/network/interfaces

iface usb0 inet static
  address 192.168.7.2
  netmask 255.255.255.252
  network 192.168.7.0
  gateway 192.168.7.1
  dns-nameservers 8.8.8.8
  dns-nameservers 8.8.4.4

iface usb1 inet static
  address 192.168.6.2
  netmask 255.255.255.252
  network 192.168.6.0
  gateway 192.168.6.1
  dns-nameservers 8.8.8.8
  dns-nameservers 8.8.4.4

debian@BeagleBone:~$ sudo route add default gw 192.168.7.1 usb0
debian@BeagleBone:~$ sudo route add default gw 192.168.6.1 usb1
```

Everytime you restart the system you need to execute Target 3rd command and Host script
```
pranab@pranab-IdeaPad-5-15ITL05:~/$sudo ./usbnet.sh

usbnet.sh    ( In Host, iptable settings to share internet between wifi and ethernet)
----------------
#!/bin/bash
##To run this script do
##1. sudo chmod +x usbnet.sh 
##2. sudo ./usbnet.sh 
sudo iptables --table nat --append POSTROUTING --out-interface wlp0s20f3 -j MASQUERADE
sudo iptables --append FORWARD --in-interface **usb0** -j ACCEPT
sudo echo 1 > /proc/sys/net/ipv4/ip_forward

**usb0** could be enx5a2a29789ba2
```

# Install kernel headers for Kernel Modules Build

`debian@BeagleBone:~$ sudo apt-get install linux-headers-'uname -r'`

# ssh connection
```
pranab@pranab-IdeaPad-5-15ITL05:~$ ssh debian@192.168.6.2
Debian GNU/Linux 11

BeagleBoard.org Debian Bullseye IoT Image 2023-08-05
Support: https://bbb.io/debian
default username:password is [debian:temppwd]

debian@192.168.6.2's password: 

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sat Aug  5 06:36:24 2023 from 192.168.6.1
debian@BeagleBone:~$ 
```

# scp protocol 

```
pranab@pranab-IdeaPad-5-15ITL05:/media/pranab/INFO/$ scp links.txt debian@192.168.6.2:/home/debian
Debian GNU/Linux 11

BeagleBoard.org Debian Bullseye IoT Image 2023-08-05
Support: https://bbb.io/debian
default username:password is [debian:temppwd]

debian@192.168.6.2's password: 
links.txt                                                                      100%  239    53.7KB/s   00:00 
pranab@pranab-IdeaPad-5-15ITL05:/media/pranab/INFO/$ 

```
