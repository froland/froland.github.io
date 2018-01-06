---
title: Installing Arch Linux on Dell XPS 15
date: 2017-07-14T09:24:12+00:00
author: froland
layout: post
categories:
  - Linux
---
My latest laptop, a Dell XPS 15 9550, is a charm. For nearly one year I&#8217;m using it now, it never failed. Its screen is gorgeous, its autonomy allows me to write software while commuting in the morning and in the evening and yet there is still plenty of juice to go on for a 2-3 hour duty during the night.

I had a previous setup with Windows 10 and WSL that was quite flexible. But even then, it never felt like the real thing. So I tried to install Ubuntu some months ago but I got disappointed. Taming the 4K display was a challenge that I was unprepared for so I switched back to Windows 10 for 6 months.

Recently, I thought it was worth giving it a new try. But this time, I wanted to test another Linux distrib I had heard of: Arch Linux. When I looked at the install manual, it reminded me of the old time when I was working with Gentoo. When every change you wanted to implement on a machine was going to take you a fair amount of time. But nevertheless, I gave it a try. And now, I&#8217;m very pleased with the achieved result. Which is why I&#8217;ll share with you how I set up my XPS 15 with dual boot windows 10 and Arch Linux.

<!--more-->

## Bootstraping

First thing first, to create yourself a USB key with Arch Linux installer. I followed the instruction on <a href="https://wiki.archlinux.org/index.php/USB_flash_installation_media" target="_blank" rel="noopener">this page</a>. It just worked without surprise.

Second, you need to get your BIOS settings right in order. The instructions are available from <a href="https://wiki.archlinux.org/index.php/Dell_XPS_15_(9550)#Pre-Installation_System_Settings" target="_blank" rel="noopener">Arch Linux WIKI page devoted to the Dell XPS 15 in the Pre-installation system settings</a>:

> Prior to installation, access the system UEFI firmware by pressing F2 during boot.
  
> Turn off legacy ROM
  
> System -> SATA: change to AHCI
  
> Secure Boot: disable
  
> POST Behavior: Fastboot: Thorough

Once this is done and you&#8217;re sure you can still boot your Windows install, you boot into Windows 10, start the Disk Management utility, and shrink the Windows partition (the latest one, DO NOT TOUCH the rescue or UEFI partitions) to make some room for Arch Linux.

You can now reboot with you USB key plugged in. _After the Arch Linux installer booted, it started to complain because it could not find my USB key (I still think this is kind of schizophrenic behaviour, given the system complaining was started &#8230; from the USB key). I had it already when trying to install Ubuntu. The simplest way I found to fix this is to unplug the USB key as soon as the complaint starts and to plug it back after 2-3 seconds. This might be just a quirk on my machine, I never got a chance to test it on another one._

Once the console starts, equip yourself with a pair of good eyes. You&#8217;re gonna need them for some time as the console font looks very tiny on my 4K display.

`<br />
# The keyboard is now configured as US qwerty. If this is not what you've got, you first need to change it.<br />
# Replace the be-latin1 with whatever you need.<br />
loadkeys be-latin1`

\# Next I setup my WiFi. As I replaced my WiFi card with an Intel one, your own device might have a different name.
  
ip link set wlp2s0 up

\# Replace YOUR\_SSID and YOUR\_PASSPHRASE in the following command with your own values.
  
wpa\_supplicant -B -i wlp2s0 -c <(wpa-passphrase &#8220;YOUR\_SSID&#8221; &#8220;YOUR_PASSWORD&#8221;)
  
dhcpcd wlp2s0

Once you&#8217;ve got a usable keyboard mapping and you&#8217;re connected on the Internet, the next step is to create partitions. I wanted to start with a very simple structure but feel free to be as creative as you want. Just remember that if you don&#8217;t use the same structure as mine, you&#8217;ll have to change the partition numbers in the following commands.

`<br />
gdisk /dev/nvme0n1<br />
# There I created a swap partition with the following key sequence: n .. 5 ..  .. +16G .. 8200<br />
# and a main partition with: n ..  ..  ..  ..<br />
# Let's not forget to write the partition table (w) and then exit gdisk.<br />
mkswap /dev/nvme0n1p5<br />
mkfs.ext4 /dev/nvme0n1p6<br />
swapon /dev/nvme0n1p5<br />
mount /dev/nvme0n1p6 /mnt<br />
# The next partition I'm gonna mount is the UEFI one so that I can use systemd-boot to dual boot.<br />
# I was a bit worried that it would ruin my Windows bootloader, but it went perfectly fine.<br />
mkdir /mnt/boot<br />
mount /dev/nvme0n1p2 /mnt/boot<br />
pacstrap /mnt base base-devel zsh vim git wpa_supplicant iw intel-ucode dialog xf86-video-intel nvidia libinput<br />
genfstab -L /mnt >> /mnt/etc/fstab<br />
arch-chroot /mnt<br />
` 

From now on, we&#8217;re in a chrooted environment. It looks like our target system, but we&#8217;re still running within the Linux kernel of the installer. What we&#8217;re going to do now is to customize everything according to our location, keymap and so on. I&#8217;m living in Belgium with an azerty keyboard and I like my Linux in English. If you have different preferences, adapt what follows.

`<br />
ln -sf /usr/share/zoneinfo/Europe/Brussels /etc/localtime<br />
hwclock --systohc</p>
<p># Uncomment the wanted locales. My own choice was en_US.UTF-8 and fr_BE.UTF-8<br />
vim /etc/locale.gen<br />
locale-gen<br />
echo "LANG=en_US.UTF-8" > /etc/locale.conf<br />
echo "KEYMAP=be-latin1" > /etc/vconsole.conf<br />
echo "YOUR_HOSTNAME" > /etc/hostname</p>
<p># Add a line with your hostname.<br />
# e.g. 127.0.1.1 YOUR_HOSTNAME.YOUR_DOMAIN YOUR_HOSTNAME<br />
vim /etc/hosts</p>
<p># Connect to your WiFi, following the wizard instructions.<br />
wifi-menu</p>
<p># Setup your root password<br />
passwd<br />
bootctl install<br />
mkdir /etc/pacman.d/hooks</p>
<p>cat << 'EOF' > /etc/pacman.d/hooks/systemd-boot.hook<br />
[Trigger]<br />
Type=Package<br />
Operation=Upgrade<br />
Target=systemd</p>
<p>[Action]<br />
Description=Updating systemd-boot...<br />
When=PostTransaction<br />
Exec=/usr/bin/bootctl update<br />
EOF</p>
<p># Write down the output of the following command as you'll need it just afterwards but won't be able to see it anymore.<br />
blkid -s PARTUUID -o value /dev/nvme0n1p6</p>
<p>cat << 'EOF' > /boot/loader/entries/arch.conf<br />
title Arch Linux<br />
linux /vmlinuz-linux<br />
initrd /intel-ucode.img<br />
initrd /initramfs-linux.img<br />
options root=PARTUUID=OUTPUT_FROM_PREVIOUS_CMD rw i915.edp_vswing=2 acpi_backlight=none acpi_rev_override=1<br />
EOF</p>
<p>echo "blacklist nouveau" > /etc/modprobe.d/nvidia.conf<br />
echo "blacklist synaptics" > /etc/modprobe.d/touchpad.conf</p>
<p>cat << 'EOF' > /etc/X11/xorg.conf.d/30-touchpad.conf<br />
Section "InputClass"<br />
Identifier "MyTouchPad"<br />
MatchIsTouchpad "on"<br />
Driver "libinput"<br />
Option "Tapping" "on"<br />
Option "Natural Scrolling" "on"<br />
EndSection<br />
EOF<br />
` 

That should be it. You can:
  
* reboot
  
* cross your fingers and hope it will boot again
  
* listen to this: [Bob Marley &#8211; Everything&#8217;s gonna be alright](https://www.google.be/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwie4PbvnYjVAhXGLVAKHQ1UA8gQyCkIKjAA&url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DOD3F7J2PeYU&usg=AFQjCNEI_tL1AN0wHViHpsVt9LFSlOM7Ww)

(I&#8217;m not sure of the sequence of these steps but, usually, I find it easier to type with my fingers not crossed.)

I will write later on how to get a GUI environment as there are some tricks here and there because of the screen.