
## Running Raspberry PI from HDD


# Partition  hard drive
Check if any partition on new disk is mounted 
```
$df
```
if there is listed any  /dev/sdaX  partition mounted , unmount them
```
$sudo parted /dev/sda
(parted) mktable msdos
```
Answer  Yes on warning that existing disk label on dev/sda will be destroyed
Make boot partition ( in Buster boot partition is increased  to 256 MB )
```
(parted) mkpart primary fat32 0% 256M
```
Make root partition
```
(parted) mkpart primary ext4 256M 100%
(parted) quit
```

Create boot filesystem ( this is not in use at the moment )
```
$sudo mkfs.vfat -n BOOT -F 32 /dev/sda1
```
Create root filesystem
```
$sudo mkfs.ext4 /dev/sda2
```
If you get question that partition is linux-swap, just ignore it an continue

## Copy  Raspbian from SD to HDD
```
$sudo mkdir /mnt/hdd
$sudo mount /dev/sda2 /mnt/hdd
ssudo mkdir /mnt/hdd/boot
$sudo mount /dev/sda1 /mnt/hdd/boot

$sudo rsync -ax --progress / /boot  /mnt/hdd
```

# Change UUID of root partition
Get PARTUUID  for root partition  on hdd
```
$sudo blkid /dev/sda2
```
This will give something like this :
```
/dev/sda2: UUID="904c10bb-1517-473d-97df-340557eae5a5" TYPE="ext4" PARTUUID="7bb35a07-02"
```
Here we need the PARTUUID
Edit /boot/cmdline.txt.
Replace value of root=PARTUUID= with value for sda2
For the example it will be root=PARTUUID=7bb35a07-02

Edit /mnt/hdd/etc/fstab
Change PARTUUID for   /  partiriton
Add entry for swap partition
Here is example of /mnt/hdd/etc/fstab for example above:
```
    proc                  /proc           proc    defaults          0       0
    PARTUUID=e08a243a-01  /boot           vfat    defaults          0       2
    PARTUUID=7bb35a07-02  /               ext4    defaults,noatime  0       1
```

# Shutdown and remove CD card
Your Raspberry PI should now boot from the HD


