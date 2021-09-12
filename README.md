# RaspberryPi-Samba-NFS
:memo:I made a NFS Server by USB drive. Now I can share the files between Linux &amp; Windows machine in the local area network.

:pushpin:`========== [Configuration] ==========`
1.	Raspberry Pi 3 Model B
2.	CPU BCM2835
3.	Memory 927MB
4.	SD card
5.	Transcend USB drive 64GB
6.	Raspbian 9

:pushpin:`========== [Procedure] ==========`
1.	Plug in the USB hard drive to one of the pi’s USB ports before you turn on the pi.
2.	Use putty to ssh your pi.
3.	Because NTFS file system is not compatible with Linux. So here we are installing a package that will allow us to read an write to NTFS file system. Type the following command into your terminal<br/>
**sudo apt-get install ntfs-3g**
4.	Linux does not make any newly attached storage device accessible by default. So we can “mount” it to a directory for use. Type the following command into your terminal<br/>
**sudo fdisk –l**<br/>
You should see all the disks on your system.
5.	Create a directory to mount the USB drive. Type the following command into your terminal<br/>
**sudo mkdir /media/USBHDD**
6.	Then “mount” the USB drive folder. Type the following command into your terminal<br/>
**sudo mount -t auto /dev/sda1 /media/USBHDD**
7.	Download Samba. Samba is a program of network file system. By installing samba, we will able to read/write to our external hard drive from any windows machine. Type the following command into your terminal<br/>
**sudo apt-get install samba samba-common-bin**
8.	Set up the smb.conf file. After Samba is installed, we need to set up the configuration file. Type the following command into your terminal<br/>
**sudo vim /etc/samba/smb.conf**
9.	Scroll down to the bottom of the file and type the following <br/>
**[Cloud]<br/>
comment = Cloud Folder<br/>
path = /media/USBHDD<br/>
valid users = @users<br/>
force group = users<br/>
create mask = 0660<br/>
directory mask = 0771<br/>
read only = no<br/>
browsealbe = yes**<br/>
10.	Create Samba user account. Type the following into your terminal<br/>
**sudo useradd cloud –m –G users**
11.	Then create a password for the user cloud. Type the following into your terminal<br/>
**sudo passwd cloud**
12.	Then set a samba password for cloud. Type the following into your terminal<br/>
**sudo smbpasswd –a cloud**
13.	Modify /etc/fstab to auto-mount the USB drive. We need to tell the pi to auto-mount the USB drive on startup, so type the following command into your terminal<br/>
**sudo vim /etc/fstab**
14.	Add the following line to the bottom of the /etc/fstab<br/>
**/dev/sda1 /media/USBHDD ntfs noatime 0 0**
15.	After all the setting up has finished. Reboot the pi and type the following into your terminal to check if the USB drive whether is mounted by system automatically or not.<br/>
**sudo lsblk**
16.	Press “windows + R”. Type the pi’s ip and share name<br/>
**\\192.168.0.109\Cloud**
17.	You can also type the following URL into web browser<br/>
**file://192.168.0.109/Cloud/**
18.	Congratulation! You've finished the NFS. Now that you can share the files between Linux & Windows machine in the local area network.
