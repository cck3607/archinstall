## Start of Install 

The first step is downloading the arch iso. It is very important that you connect this iso via the setting and cd/disk image. Should also boot in UEFI mode. Once connected the first thing you should do is check you internet connect. The first iso I downloaded did not allow me to connect to the internet after a few hours of trying to fix it I finally downloaded a different iso that connected to the internet right away. 

### Once inside the VM

After gaining access to the VM with your iso follow these steps and enter them in order to install arch.

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

##Select Mirrors
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup
## then update the mirror list file with 10 mirrors by download speed
reflector --verbose --latest 10 --sort rate --save /etc/pacman.d/mirrorlist

##Install Arch Linux Base System
pacstrap /mnt/ base linux linux-firmware net-tools networkmanager openssh nano 
###you can use nano or vi. Here is where you would install any other tools or packages you want 

##Create fstab
genfstab -U /mnt >> /mnt/etc/fstab
###Verify the fstab entries using the below command.
cat /mnt/etc/fstab

##Arch Linux System Configuration 
arch-chroot /mnt ###chroot to the new system

##Set System Language 
###You can configure the system language by uncommenting the required languages from /etc/locale.gen file
nano /etc/locale.gen
###Uncomment en_US.UTF-8 UTF-8 for American-English and then generate locales by running
###control x to save and exit
locale-gen
###Set the LANG variable in /etc/locale.conf file
echo "LANG=en_US.UTF-8"  > /etc/locale.conf

##Set Timezone
###configure the system time zone by creating a symlink of your timezone to the /etc/localtime file
ln -sf /usr/share/zoneinfo/US/Central /etc/localtime 
###all the available timezones are found under /usr/share/zoneinfo directory
###set the hardware clock to UTC.
hwclock --systohc --utc

##Set Hostname
###Place the system hostname in /etc/hostname file
echo "archlinux-2021.connor.local" > /etc/hostname

##Set Root Password
passwd

##Install Grub Bootloader
###make sure you are still in arch-chroot
pacman -S grub efibootmgr
###Create the directory where the EFI partition will be mounted
mkdir /boot/efi
###mount the esp partition just created
mount /dev/sda1 /boot/efi
###install grub with the command below be careful with spaces
grub-install --target=x86_64-efi --bootloader-id=GRUB --efi-directory=/boot/efi
###last step below 
grub-mkconfig -o /boot/grub/grub.cfg

##Install Desktop Enviroment(GNOME in this case)
###here is where you can reboot, I found it easier to install gnome before reboot
pacman -S xorg
###now install GNOME dektop enviroment on arch linux using
pacman -S gnome
###now enable display manager for GDM for arch as well as enabling network manager
systemctl start gdm.service
systemctl enable gdm.service
systemctl enable NetworkManager.service
###now exit from chroot with exit command
exit
###then shutdowm system and reboot
shutdown now

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/cck3607/archinstall/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
