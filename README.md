# linux-server
A cheat sheet for setting up a simple linux server using a samba shared directory

# Steps

### Configure Wifi (Ubuntu Server and Raspberry Pi 4)
* Flash image onto SD card using [Raspberry Pi Imager](https://www.raspberrypi.org/software/)
* Eject SD card, then connect back to PC you used to flash
* Open network-config
* Add your network info under `access-points:`

### Create a folder to mount a drive to and share
`sudo mkdir /mnt/ssd`

### If the drive is already mounted use the line below to unmount, so you can remount to another directory
`sudo umount /dev/sda1`

### Install ntfs-3g if using an ntfs formatted drive
`sudo apt install ntfs-3g`

### Mount the drive to your share folder
`sudo mount -t ntfs-3g /dev/sda1 /mnt/ssd/`

### Find UUID for the drive
`sudo blkid`

### Add drive to fstab for auto mounting
`sudo nano /etc/fstab`

### Add this line to the fstab file at the very end. Replace the X's with the UUID that you found using blkid
`UUID=XXXXXXXXXXXXXXXX  /mnt/ssd  ntfs  defaults  0  2`

### Install Samba
`sudo apt-get install samba-client samba-common -y`

### Open `smb.conf` and add share properties
`sudo nano /etc/samba/smb.conf`

### Add this text to the very end of smb.conf
```
[share]  
comment = pi share
path = /mnt/ssd
read only = no
browsable = yes
```

### Create a password for the Samba share
`sudo smbpasswd -a pi`
