[![Donate via Paypal.me](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://paypal.me/djbp)

# How to install Debian 10.1 "buster" on your Dell XPS 15 9560

Debian for me as always been my favourite Linux distro. Where every other mainstream distro has required consistent debugging, [16 plus boot flags](https://forum.mxlinux.org/viewtopic.php?t=47931), endless "tweaking" to prevent freezes and kernel panics and no less than 1000 cups of coffee to stay sane. Debian just "works" basically right out of the box. Therefore it's no surprise that installing Debian on my Dell XPS 9560 (Touchscreen 4K variant) was a total piece of cake.

Hopefully this "guide" can help some people in getting this marvelous distro to work on their Dell.

# My Laptop Specs

* CPU: Intel® Core™ i5-7300HQ CPU @ 2.50GHz × 4
* RAM: 16GB DDR4 Kingston HyperX *(replaced the Hynix RAM)
* HDD: SK Hynix 256GB Nvme SSD
* GFX: Nvidia GTX 1050
* WLAN: Intel AC 9260 *(I ditched the Broadcom Killer Wireless immediately)
* PANEL: 4K 3840x2160 Touchscreen

# Pre-Installation Requirements

If it somehow wasn't obvious. You'll need to change a few BIOS settings on your Dell. Firstly, make sure your BIOS is updated to the latest version. You can do this on Windows 10 using the [SupportAssist app](https://www.dell.com/support/contents/us/en/04/article/product-support/self-support-knowledgebase/software-and-downloads/SupportAssist) which can be downloaded from the Windows Store (run a full check and install updates). You can also update the BIOS from Dell's site [here](https://www.dell.com/support/home/us/en/04/drivers/driversdetails?driverid=yf1g0&lwp=rt). Make sure to double check that you're installing the latest BIOS version.

The BIOS settings we need to change are:

1. Set SATA mode to AHCI
2. Disable "Secure Boot"
3. Check if Windows 10 is installed in UEFI mode or Legacy

Setting SATA mode to AHCI will make it impossible to boot back into Windows 10. However, there is a completely painless fix to this that takes about 3 minutes. You can read it [here](http://triplescomputers.com/blog/uncategorized/solution-switch-windows-10-from-raidide-to-ahci-operation/). Follow those steps completely to ensure that your Windows installation works after changing from RAID mode to AHCI mode in the BIOS. Skip this step if you don't have Windows 10 installed, are planning to remove it completely or don't want to dual boot.

Next just disable Secure Boot and then do step 3 quickly.
Nothing else needs to be done here and you should now be ready to install Debian.

# Installation

Debian (obviously) doesn't like non-free drivers and software and it provides very limited support for them. So the first problem I had was using the 'netinst' Debian .iso which requires internet as it requests the missing Intel 9260 Wi-Fi drivers during the installation which I couldn't be bothered to look for and provide.

Another option here would be just using a third-party Thunderbolt to Ethernet adapter that I have which works OOTB, but I didn't know where it was. Therefore I used [a full 'live' .iso](https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/) to install Debian. Here I picked Gnome because I find it's the perfect balance between performance and style and I plan on changing the theme later.

I used [BalenaEtcher](https://www.balena.io/etcher/) to make the installation USB, but [Rufus](https://rufus.ie/) also works. At first I tried to use a Corsair GTX 3.1 256GB USB-SSD but Debian kept complaining that the 'CDROM' media couldn't be found and addtional necessary files were missing. So I wound up having to go with a much older Lexar 32GB USB 3.0 drive which worked without any issues. If you get the same sort of 'CDROM' error, just use a different USB drive.

Install Debian with the locale and specific settings that you require. I chose to resize and partition my SSD and give Debian about 50GB to run it alongside Windows 10, as I'll be using Debian just for programming and some Linux software. Make sure that if you want to dual-boot Windows 10, you are sure if Windows is installed in UEFI mode or Legacy. When asked to "force UEFI" installation you can do so if Windows 10 is also installed in UEFI mode (it is by default on this laptop, but check first to be sure!)

# Post-Installation Requirements

On first run Debian will try to use the open source noveau graphics drivers and you'll probably end up getting a static cursor and black screen. We can fix this easily by booting with the single simple boot flag of: `nouveau.modeset=0`
On the grub screen just press 'e' and add that boot flag at the end of the 'linux' line and one 'space' after the word 'quiet'. Then press Ctrl+X to boot. Debian should now boot to your desktop environment without any additional requirements.

# Getting Wi-Fi to work

You'll probably have the Broadcom WiFi card, in which case I can't help you with how to get that working (if it doesn't already) but I'm sure a simple Google search for "Broadcom WiFi Debian" wil be enough to solve that.

If you have the Intel AC 9260 like me though, just download the drivers from [here](https://www.intel.com/content/www/us/en/support/articles/000005511/network-and-io/wireless-networking.html) read the included readme.md and copy the file to the correct directory and then also follow a few more steps to have the Wi-Fi work automatically at boot (Big thanks to 'dperezvi' for this info).

Install the following packages with apt install:
* ```firmware-iwlwifi```
* ```intel-microcode``` (non-free) and
* ```firmware-misc-nonfree```

Reboot (remember the `nouveau.modeset=0` flag). Wi-Fi should work fine now at each boot.

# Installing Nvidia Graphics

Follow the Wiki page [here](https://wiki.debian.org/NvidiaGraphicsDrivers) using nvidia-detect. Install nvidia-drivers and then simply reboot. Now you don't need the boot flag any more.

That's it. Absolutely painless compared to other distros that require a mountain of work to be even remotely stable. With Wi-Fi we can install anything else we need and the nvidia-drivers should automatically prevent any screen tearing when scrolling making using Debian completely painless from now.

Go forth and enjoy Debian!

Todo for later in this guide:

1. Reduce battery drain from ~13mW to ~6mW by fully disabling the dedicated Nvidia GTX 1050 (I won't use it on Linux).
