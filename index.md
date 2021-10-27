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

```
## Creating Partitions 
```markdown
fdisk -l 
###hard disk should be labled /dev/sda
fdisk /dev/sda
###should see command (m for help) enter p 
p 
### then create a new partition with n
n 
### then set primary partition with command below 
p 
### enter 1 to set to first partition 
1 
### for first sector just leave blank and hit enter for default
press enter 
###now set size for last sector to command below
+1024M  
## Creating SWAP Partition Disk
### then create a new partition with n
n 
### then set primary partition with command below 
p 
### enter 2 to set to 2nd partition 
2 
### for first sector just leave blank and hit enter for default
press enter 
###now set size for last sector to command below
+4G 
### change partition type with command below
t 
###2nd partition==Swap
2 
### enter 82==swap
82 

## Creating /(root) Partition Disk
### now create a new partition with n again
n 
### then set primary partition with command below 
p 
### enter 3 to set to 3rd partition 
3 
### for first sector just leave blank and hit enter for default
press enter 
### leave blank again to use remaining space /
press enter 
## Once you have created partitions, use p to confirm the creation of partitions and then, w to save the changes.
```

## Creating Filesystem 
```markdown
mkfs.fat -F32 /dev/sda1
mkfs.ext4 /dev/sda3
mkswp /dev/sda2

##Mount partitions 
mount /dev/sda3 /mnt
mkdir /mnt/efi
mount /dev/sda1 /mnt/efi
swapon /dev/sda2
```
## Selecting Mirrors
```markdown
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup
## then update the mirror list file with 10 mirrors by download speed
reflector --verbose --latest 10 --sort rate --save /etc/pacman.d/mirrorlist
```
## Installing Arch Linux Base System
```markdown
pacstrap /mnt/ base linux linux-firmware net-tools networkmanager openssh nano 
###you can use nano or vi. Here is where you would install any other tools or packages you want 
```
## Creating fstab
```markdown
genfstab -U /mnt >> /mnt/etc/fstab
###Verify the fstab entries using the below command.
cat /mnt/etc/fstab
```
## Arch Linux System Configuration
```markdown
###chroot to the new system
arch-chroot /mnt 
```
## Setting system language
```markdown
###You can configure the system language by uncommenting the required languages from /etc/locale.gen file
nano /etc/locale.gen
###Uncomment en_US.UTF-8 UTF-8 for American-English and then generate locales by running
###control x to save and exit
locale-gen
###Set the LANG variable in /etc/locale.conf file
echo "LANG=en_US.UTF-8"  > /etc/locale.conf
```
## Selecting Timezone
```markdown
###configure the system time zone by creating a symlink of your timezone to the /etc/localtime file
ln -sf /usr/share/zoneinfo/US/Central /etc/localtime 
###all the available timezones are found under /usr/share/zoneinfo directory
###set the hardware clock to UTC.
hwclock --systohc --utc
```
## Selecting Hostname 
```markdown
###Place the system hostname in /etc/hostname file
echo "archlinux-2021.connor.local" > /etc/hostname
```
```markdown
## Setting Root Password
passwd
```
## Installing Grub Bootloader

```markdown
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
```
## Installing Desktop Enviroment(GNOME in this case)
```markdown
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
shutdwn now
```



### After installing 
You should now be in the desktop enviroment. Here you must login as root with the password we set earlier.
## Creating users
```markdown
#enter the command below to create user connor 
sudo useradd connor 
#you may prompted to enter root password
#enter the command below to create user sal
sudo useradd sal
#enter the command below to create user codi
sudo useradd codi
```
## Setting Password For Users
```markdown
#to set password for user connor enter command below
sudo passwd connor 
#then enter desired password 
#to set password for user sal enter command below
sudo passwd sal
#then enter desired password 
#to set password for user codi enter command below
sudo passwd codi
```
## Granting Sudo Permissions For a User
```markdown
#enter the command below to grant user connor sudo access
sudo usermod -a -G wheel connor 
#enter the command below to grant user sal sudo access
sudo usermod -a -G wheel sal 
#enter the command below to grant user codi sudo access
sudo usermod -a -G wheel codi

##Editing Wheel to allow for sudoers 
##first we must edit /etc/sudoers 
nano /etc/sudoers
#then find the line that begins with #wheel and delete the #
#control x to save and exit
```
## Changing Shell
```markdown
##to change the to zsh enter the command below
chsh -s /bin/zsh
```
##Adding alias in shell
```markdown
#enter command below to add alias to clear
alias c='clear
#enter command below to add alias to history 
alias h='history'
alias j='jobs -l'
```
##Adding Color to Shell
```markdown
#Enter the command below to change color to red
PS1="\033[31m[\u@\h \w]\$ \033[0m"

```
