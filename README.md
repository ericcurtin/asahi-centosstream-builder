# asahi-centosstream-builder

Builds a minimal CentOS Stream 9 image to run on Apple M1 systems. Kudos to Leif Liddy and Asahi community for helping make this happen.

## package install for building images

```dnf install mkosi arch-install-scripts systemd-container zip qemu-user-static```  

note: ```qemu-user-static``` is not needed if building the image on an ```aarch64``` system.   

note: at present building the arch-install-scripts is only available on fedora 35/36

## To install a prebuilt image

Make sure to update your macOS to version 12.3 or later, then just pull up a Terminal in macOS and paste in this command:

```
curl https://ecurtin.fedorapeople.org/centos.sh | sh
```

**Notes:** 
1. The root password is **centosstream**
2. On the first boot the ```asahi-firstboot.service``` will run and will take around ```45 seconds``` to complete.  
   Do not shutdown or reboot the system before this service has completed.  
3. The Asahi Linux-related RPM's (and Source RPM's) used in this image can be found here:  
   https://ecurtin.fedorapeople.org/asahi-linux/36/  
   All RPM's signed are signed by a GPG key.  
   The repo config can be found here:   
   https://ecurtin.fedorapeople.org/asahi-linux/asahi-linux.repo  
4. The CentOS Stream kernel config used is nearly identical to the kernel config used by the Asahi Linux project (a few CentOS Stream specific modifications were made):

   https://github.com/AsahiLinux/PKGBUILDs/blob/main/linux-asahi/config
5. ```systemd-networkd``` is the sole network service that's installed in this image.  
   Basic config files for the ```eth0``` and ```wlp1s0f0``` interfaces are included in the image   
   ie.  
   **/etc/systemd/network/eth0.network**
   ```
   [Match]
   Name=eth0

   [Network]
   DHCP=yes
   ```
   The ```eth0``` interface is what an external usb ethernet adapter "should" be assigned to.   
6. Use ```iwd``` to setup the wifi interface (see info below)   

## Wiping Linux

Bring up a Terminal in macOS and run the following Asahi Linux script:  
```curl -L https://alx.sh/wipe-linux | sh```  
You should definitely understand what this script does before running it.  
You can find more info here:  
https://github.com/AsahiLinux/docs/wiki/Partitioning-cheatsheet 

