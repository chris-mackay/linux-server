# linux-server
A cheat sheet for setting up a simple linux server using a samba shared directory

## Raspberry pi 4 (2gb RAM), Ubuntu Server 20.04

</br>
</br>

# Steps

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

</br>
</br>

# Setup Wifi

### Install network tools
`sudo apt install net-tools` </br>
`sudo apt install wireless-tools`

### Install wpasupplicant
`sudo apt install wpasupplicant`

### Find your wireless interface name (mine was wlan0)
`ifconfig -a`

### Find your wifi network 
`sudo iwlist wlan0 scan | grep EESID`

### Add your wifi network to `wpa_supplicant.conf`
`wpa_passphrase "EESID" "Password" | sudo tee /etc/wpa_supplicant.conf`

### Associate wifi network with interface
`sudo wpa_supplicant -c /etc/wpa_supplicant.conf -i wlan0`

### Check if your wifi interface has been assigned an IP address, if not keep trying the command above until it does.
`ifconfig -a`