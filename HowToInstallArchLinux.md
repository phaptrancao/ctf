# How to install Arch Linux
## 20/1/2017

## Part 1 Of 3: Starting The Setup Process

### 1. Download the Arch installation image

Go to www.archlinux.org and download the Arch Linux iso

### 2. Burn the iso to a blank disc

#### a. On Linux

Type `sudo dd bs=4M if='path_to_your_iso' of='path_to_your_usb' status=progress && sync`

Example: asume your usb is in `/dev/sdb` and your `iso` image in your `home`, then type this

```bash
sudo dd bs=4M if='/home/username/ArchLinux.iso' of='/dev/sdb' status=progress  && sync
```

#### b. On Windows

Use [rufus](https://rufus.akeo.ie) or [UltraISO](https://www.ezbsystems.com/ultraiso) to burn your iso to your installation media

#### c. On macOS

Plug your usb into your mac

In terminal, type this : `diskutil list`. Then check your usb from the output using the amount of data. Let assume it is `/dev/disk3`

Then unmount the usb : `diskutil unmountDisk /dev/disk3`

And run this command: `sudo dd if='path_to_your_iso' of='/dev/disk3' bs=1M && sync`

### 3. Backup any data on your computer or the drive you are installing arch linux on.

### 4. Insert the installation media to your computer

### 5. Press the key that allows you to change the boot order

On most newer computers, this is `F12`. through the exact key should be displayed on the screen during boot up.

### 6. Select `Boot from Arch Linux (x86_64)`

If you are booting in UEFI mode then choose `Archiso x86_64 UEFI`

### 7. Test internet connection

Before continuing, you must need internet connection in order to install Arch Linux. I recommend that it should be an Ethernet or LAN network.

Type `ping -c 3 google.com` to test your network. If it is not working, ensure that your Ethernet cable is securely connected or you are close enough to your wireless router to get a good signal.

If it works, then run this command `ping www.google.com > /dev/null 2>&1 &`, it will keep your internet going through

If you have an **authoried internet access**, you should have to log in to use it. If the login page need `javascript` to login, don't worry! The system on installation medium will have `elinks`. **Elinks** - a terminal-based browser will help you. Please follow these guides:

+ type `elinks archlinux.org`

+ press the `Esc` key. Press `o` to open `Options manager`

+ move the cursor to `ECMAScripts` and press the space bar, select the `Enable` field and use right arrow to move to `Edit` option

+ type `1` to the value and use the down arrow key to `OK`

+ press the right arrow key to move to `Save` option and choose `OK`

+ press `g` and type `archlinux.org` or anything else to access the login page. The else part is yours.

## Part 2 Of 3: Set Up Your Partitions

> You can use Linux Live CD like `Ubuntu` and software `GParted` on it or with `Disk Management` on `Windows` to partition the hard disk. But I discourage this way. If you do, follow these:
	
+ type `lsblk` to see the name of your partitions, like `/dev/sda1`. And make note the partitions you use for Arch

+ We use at least 3 partitions here - one for the OS, one for `/home` and one for `swap`. You could also modify more partitions meet your need like for `/boot`, etc... **or** remove `/home` partition, just `root` partition is enough for beginners.

+ Sometimes you will have `/dev/sda` as your USB installation media and `/dev/sdb` as your main hard drive, the drive you use to install `Arch`. But in this toturial, I will assume that `/dev/sda` is the disk you want to install Arch Linux.

+ If you want to erase all your disk to install only Arch Linux, type `sgdisk --zap-all /dev/sda`

### 1. Check if you are using a UEFI motherboard.

Type `ls /sys/firmware/efi/efivars`, if the result is `No such file or directory` then your boot mode is `Legacy BIOS`.

### 2. Create GPT partition (only `UEFI`)

You will need to create an extra EFI partition as well. In this example, we will be creating a Root partition, a Swap partition, a Home partition, and an EFI partition

+ Start `cgdisk` by typing `cgdisk /dev/sda` and press `Enter`

+ Select `New` and then press `Enter` to select first sector, type the size you want for Root partition, I recommend it as 25G. After that, press `Enter` 2 times to create it.

+ Press down arrow key until the remaining free space is selected. Press `Enter` to select first sector, type the size you want for Swap partition, a half of your RAM. Then also press `Enter` 2 times to create it.

+ Press down arrow key until the remaining free space is selected. Press `Enter` to select first sector, type the size you want for Home partition. I think it should be minimum as 20G but make sure to leave at least 500MB for your EFI partition. Then also press `Enter` 2 times to create it.

+ Press down arrow key until the remaining free space is selected. Press `Enter` to select first sector, type the size you want for EFI partition, Make sure to leave at least 500MB for your EFI partition. Then also press `Enter` 2 times to create it.

+ Select `Write` to write the new partition to the disk. Type `yes` and choose `Quit` to exit `cgdisk`.

### 3. Create MBR partition (only `Legacy BIOS`)

Type `cfdisk /dev/sda`

I am going to use `cfdisk` to set up 3 partitions :

+ Root partition, `/dev/sda1` as primary bootable with size of 25G and ext4 formatted

+ Swap partition, `/dev/sda2` as primary with size half of your RAM

+ Logical partition, `/dev/sda5` as your '/home', rest of the space and ext4 formatted.

#### Step 1. Create primary partition

Select **New**, enter partition size `25G` in my case

Then, select **Type**, choose `Linux` format (alias for ext4 format)

Next, choose `Bootable` to make this partition as bootable partition

Then, select *Write* using left/right arrow key to write partition changes. Type **Yes** to save the changes.

#### Step 2. Create `swap` partition

Select **New**, enter partition size `2G` in my case, remember it is half of you RAM

Then, select **Type**, choose `Linux Swap/Solaris` format

Then, select *Write* using left/right arrow key to write partition changes. Type **Yes** to save the changes.

#### Step 3. Create logical (extended) partition

Select **New**, enter partition size. Since it is the last partition, I just press enter to select rest of the disk space for this partition.

Then, select **Type**, choose `Linux extended` format

Then, select the free space after it and select **New**

Then, select **Type**, choose `Linux` format (alias for `ext4` format)

Then, select *Write* using left/right arrow key to write partition changes. Type **Yes** to save the changes.

> After creating the necessary partitions, select `Quit` option and exit the partition manager. You can use `lsblk` or `fdisk -l` to verify your partition details

### 4. Format your partitions

> Remember this step will delete all data in these partitions, so if you already have a partition such as `/home` on your old `Ubuntu` system and want to mount it, jump to part 5

```bash
# format `/` partition
mkfs.ext4 /dev/sda1
# format `/home` partition
mkfs.ext4 /dev/sda5
# format `swap` partition
mkswap /dev/sda2
# activate the `swap`
swapon /dev/sda2
#
# only for GPT partition
#
# format `EFI` partition
mkfs.ext4 /dev/sda7 # assuming `sda7` is your `EFI` partition
#                   # you `lsblk` to find your partitions
```

### 5. Mount your partitions

Type the following commands:

```bash
# create /home and /mnt directory for mounting
mkdir -p /mnt/home
# mount your `/` directory
mount /dev/sda1 /mnt
# mount your `/home` directory
mount /dev/sda5 /mnt/home
#
# Only for GPT partition
#
mkdir /mnt/boot
mount /dev/sda7 /mnt/boot
```

## Part 3 of 3: Installing Arch Linux

### 1. Select the mirrors

You could also visit [this page](http://archlinux.org/mirrorlist) on another computer and use the tool to find the closest mirror to your physical location. Copy the address down. It may help to write down a few mirrors in case one is offline.

The your mission is *change the first* `Server =` line in `/etc/pacman.d/mirrorlist` to your new primary mirror.

Use `vi /etc/pacman.d/mirrorlist`. 

Scroll down to your preferred mirror (the closer to your location the better), press `yy` to copy. 

Then scroll back up and press `p` to paste the line at the top of the list.

Then press `Esc` and type `:x` to save and exit the `vi` program.

### 2. Install the Arch Base System

Type the following command

```bash
pacstrap -i /mnt base base-devel
```

And wait until finishing. The waiting time depends on your Internet speed.

### 3. Create an `fstab` file (file system table)

This file allows Arch to identify your partitions's file systems. Type:

`genfstab -U -p /mnt >> /mnt/ect/fstab`

Then verify the `fstab` entries using the output of `cat /mnt/etc/fstab` and `blkid`

### 4. Access your newly-installed Operating System. 

Type : `arch-chroot /mnt /bin/bash`

### 5. Edit your Locale file

First, install `vim` by `pacman -S vim` and `ln -fs /usr/bin/vim /usr/bin/vi`

Type `vi /etc/locale.gen`. Uncommment `en_US.UTF-8 UTF-8` by the `x` command. Save and exit the file by press `ESC` and type `:x`

Then generate the new locale using : `locale-gen`

Type `echo LANG=en_US.UTF-8 > /etc/locale.conf` and `export LANG=en_US.UTF-8`

### 6. Set your timezone and hardware clock

Type : `ln -s /usr/share/zoneinfo/Asia/Ho_Chi_Minh /etc/localtime`

Then set the time standard to UTC using : `hwclock --systohc --utc`

### 7. Set your hostname

This is the name that your computer will display when connected to the network. Replace ***myhostname***  in the following commands with your own hostname

Type : `echo -n myhostname > /etc/hostname`

Then edit `/etc/hosts` file with your hostname like this

```
127.0.0.1	localhost
127.0.1.1	myhostname

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

### 8. Configure your network (only for wired)

Skip if you have a wireless network connection

Type `systemctl enable dhcpcd`

### 9. Configure your network (only for wireless)

Type the following commands to enable it : 

```bash
pacman -S iw wireless_tools wpa_supplicant wpa_actiond dialog
systemctl enable netctl-auto
```

Next time you reboot, type `wifi-menu` to access the wireless menu

### 10. Configure your package manager

Type `vi /etc/pacman.conf`. Scroll down and uncomment these line

	[multilib]
	Include = /etc/pacman.d/mirrorlist

Save changes and exit.

After that, type `pacman -Sy` to refresh your reposity list.

### 11. Create a normal user

Because being `root` user when using normal task is dangerous, you could distroy your system by mistake when you are `root` user.

Replace myusername with your username, type the following commands

```bash
useradd -m -g users -G wheel,storage,power -s /bin/bash myusername
passwd myusername
```

Now install the `sudo` package, `sudo` will let you have `superuser` privilege when you need. Type these commands:

```bash
pacman -S sudo
visudo
```

Scroll down and uncomment this line: `%wheel ALL=(ALL) ALL`. Save and exit the editor.

### 12. Configure the bootloader

This is the software that loads the OS when computer starts.

If you're doing **dual boot**, type this : `pacman -S os-prober`

#### Only UEFI

Type these commands:

```bash
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=arch-grub --recheck /dev/sda
```

#### Only Legacy BIOS

Type these commands:

```bash
pacman -S grub-bios
grub-install --target=i386-pc --recheck /dev/sda
```

#### Both UEFI and BIOS

Type these commands:

```bash
cp /usr/share/locale/en\@quot/LC-MESSAGES/grub.mo /boot/grub/locale/en.mo
grub-mkconfig -o /boot/grub/grub.cfg
```

### 13. Reboot your computer

Then, exit from chroot, unmount partitions and reboot you computer.

Type:

```bash
exit
umount -R /mnt
reboot
```

Remember to remove the installation media at this time.

### 14. Sign in after Arch boots up

Use the password you created as a normal user to log in.

### 15. Install GUI : Get Your Desktop Up And Running (XFCE4 as default)

#### 1. Install the X window system

`sudo pacman -S xorg-server xorg-xinit xorg-server-utils`

#### 2. Get the 3D working

`sudo pacman -S mesa`

#### 3. Install the fonts you will need

`sudo pacman -S ttf-dejavu noto-fonts-cjk noto-fonts`

If you want to install Emoji: `sudo pacman -S noto-fonts-emoji`

#### 4. Enable `AUR` reposity

`AUR` is one of the best of Arch Linux with community maintain packages, type this to enable it: `sudo pacman -S yaourt` 

#### 5. If you want to install Microsoft font

(Do not type `sudo` on this command) `yaourt -S ttf-ms-fonts` and following the guides after that.

#### 6. Install your network manager

Type this : `sudo pacman -S networkmanager network-manager-applet` and `sudo dhcpcd enable NetworkManager`

#### 7. If you have trouble with `Wi-Fi`

Google for install `wifi driver for arch`

#### 8. Disable `root` user account

For safe, type `sudo passwd -dl root`

#### 9. Install XFCE4 Dekstop

`sudo pacman -S xfce4 lxdm` and type `1 2 3 4 5 6 8 9 10 11 12 13 14 16`

Then type `sudo vi /etc/lxdm/lxdm.conf` and change line `session=/usr/bin/startlxde` to `session=/usr/bin/startxfce4`. Save change and exit.

Then, type `sudo systemctl enable lxdm` and reboot your system with `reboot`
