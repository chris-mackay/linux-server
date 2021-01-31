# linux-server
Steps for setting up a linux server using a samba shared directory

Raspberry Pi 4 (2gb RAM), Raspberry Pi OS (32-bit)

</br>
</br>

# Steps

### Create a folder to mount a drive to for sharing
`sudo mkdir /mnt/ssd`

### Find your connected drive name and UUID (mine was sda1)
`blkid`
</br>
</br>

![blkid](blkid.png?raw=true "blkid")

### If the drive is already mounted use the line below to unmount, so you can remount to another directory
`sudo umount /dev/sda1`

### Install ntfs-3g if using an ntfs formatted drive
`sudo apt install ntfs-3g`

### Mount the drive to your share folder
`sudo mount -t ntfs-3g /dev/sda1 /mnt/ssd/`

### Add drive to fstab for auto mounting
`sudo nano /etc/fstab`

### Add this line to the fstab file at the very end. Replace the X's with the UUID that you found using blkid
`UUID=XXXXXXXXXXXXXXXX  /mnt/ssd  ntfs  defaults  0  2`

</br>

![fstab](fstab.png?raw=true "fstab")

### Install Samba
`sudo apt-get install samba`

### Open `smb.conf` and add share properties
`sudo nano /etc/samba/smb.conf`

### Add this text to the very end of smb.conf
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
`sudo smbpasswd -a pi`

### Reboot
`sudo reboot`

</br>
</br>


# Setup Wifi (if necessary)

### Install network tools
`sudo apt install net-tools` </br>
`sudo apt install wireless-tools`

### Install wpasupplicant
`sudo apt install wpasupplicant`

### Find your wireless interface name (mine was wlan0)
`ifconfig -a`

### Find your wifi network 
`sudo iwlist wlan0 scan | grep EESID`

### Add your wifi network to wpa_supplicant.conf
`wpa_passphrase "EESID" "Password" | sudo tee /etc/wpa_supplicant.conf`

### Associate wifi network with interface
`sudo wpa_supplicant -c /etc/wpa_supplicant.conf -i wlan0`

### Check if your wifi interface has been assigned an IP address.
`ifconfig -a`

</br>
</br>

# Connect to server from another device

### Find the IP address assigned to your Pi
`ifconfig -a`

</br>

![ifconfig](ifconfig.png?raw=true "ifconfig")

### Windows (Map Network Drive)
`\\10.0.0.114\[share]` Include brackets

### Linux, MacOS, iPhone
`smb://10.0.0.114`