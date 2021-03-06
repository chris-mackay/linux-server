# linux-server
Steps for setting up a linux server using a samba shared directory and a connected drive. The steps below assume you already have a linux operating system installed on your Raspberry Pi and a drive connected that you want to share on your network.
</br>
</br>

Configuration used to create these steps:

### Raspberry Pi 4 (2gb RAM)
### Raspberry Pi OS (32-bit)
### Samsung SSD 860 EVO 250GB 2.5 Inch SATA

</br>

# Steps

### Open a terminal window
</br>

### Create a folder to mount a drive to for sharing
`sudo mkdir /mnt/ssd`

### Find your connected drive name and UUID (mine was sda1)
`blkid`
</br>
</br>

![blkid](blkid.png?raw=true "blkid")

### If the drive is already mounted use the line below to unmount, so you can remount to another directory
`sudo umount /dev/sda1`

### Install ntfs-3g if using an NTFS formatted drive
`sudo apt-get install ntfs-3g`

### Install exfat utils if using an exFAT formatted drive
`sudo apt-get install exfat-fuse` </br>
`sudo apt-get install exfat-utils`

### Mount the drive to your share folder (NTFS)
`sudo mount -t ntfs-3g /dev/sda1 /mnt/ssd/`

### Mount the drive to your share folder (exFAT)
`sudo mount -t exfat /dev/sda1 /mnt/ssd/`

### Add drive to fstab for auto mounting
`sudo nano /etc/fstab`

### Add this line to the fstab file at the very end. Replace the X's with the UUID that you found using blkid. If using an exFAT formatted drive, replace ntfs with exfat.
`UUID=XXXXXXXXXXXXXXXX  /mnt/ssd  ntfs  defaults  0  2`

</br>

![fstab](fstab.png?raw=true "fstab")

### Install Samba
`sudo apt-get install samba`

### Open `smb.conf` and add share properties
`sudo nano /etc/samba/smb.conf`

### Add this text to the very end of `smb.conf`
```
[share]
comment = pi share
public = yes
writeable = yes
browseable = yes
path = /mnt/ssd
create mask = 0777
directory mask = 0777
guest ok = yes
only guest = no
```

### Create a password for the Samba share
This is the password you will use when connecting to your server from another device

`sudo smbpasswd -a pi`

### Reboot
`sudo reboot`

</br>
</br>

# Connect to server from another device

### Find the IP address assigned to your Pi
`ifconfig -a`

</br>

![ifconfig](ifconfig.png?raw=true "ifconfig")

</br>

## Windows
</br>

![map_drive](map_drive.png?raw=true "map_drive")

</br>

<kbd>![connect_windows](connect_windows.png?raw=true "connect_windows")</kbd>

</br>

![pass_windows](pass_windows.png?raw=true "pass_windows")

</br>

## iPhone (Files app)
</br>

<kbd>![browse_iphonee](browse_iphone.png?raw=true "browse_iphone")</kbd>

</br>

<kbd>![connect_iphone](connect_iphone.png?raw=true "connect_iphone")</kbd>

</br>

<kbd>![pass_iphone](pass_iphone.png?raw=true "pass_iphone")</kbd>
