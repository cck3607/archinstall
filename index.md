## Start of Install 

The first step is downloading the arch iso. It is very important that you connect this iso via the setting and cd/disk image. Should also boot in UEFI mode. Once connected the first thing you should do is check you internet connect. The first iso I downloaded did not allow me to connect to the internet after a few hours of trying to fix it I finally downloaded a different iso that connected to the internet right away. 

### Once inside the VM

After gaining access to the VM with your iso follow these steps.

```markdown


# Confirming internet 
ip addr show
ping google.com
## Set The Console Keyborad Layout
ls /usr/share/kbd/keymaps/**/*.map.gz 
## Verify The Boot Mode
ls /sys/firmware/efi/efivars
## Update The System Clock
timedatectl set-ntp true
### check time with 
timedatectl status
## Creating EFI Partition Disk
fdisk -l ###hard disk should be labled /dev/sda
fdisk /dev/sda
p ###should see command (m for help) enter p 
n ### new partition 
p ### primary partition 
1 ### first partition 
press enter ### for first sector 
+1024M ###set size for last sector 
## Creating SWAP Partition Disk
n ### new partition
p ### primary partition
2 ### 2nd partition
press enter ### for first sector 
+4G ###set size for last sector
t ### change partition type 
2 ###2nd partition==Swap
82 ### 82==swap
## Creating /(root) Partition Disk
n ### new partition
p ### primary partition
3 ### 3rd partition
press enter ### to use default start sector
press enter again ### to use remaining space /
## Once you have created partitions, use p to confirm the creation of partitions and then, w to save the changes.

## Create Filesystem 

mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda3
mkswp /dev/sda2

##Mount partitions 
mount /dev/sda3 /mnt

mkdir /mnt/efi

mount /dev/sda1 /mnt/efi

swapon /dev/sda2


[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/cck3607/archinstall/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
