### elementary OS on a Mac (using rEFInd & EFI stub loading)

**NOTE**: I'm making a few assumptions about the way your computer is set up. Namely:

#### I assume your ESP partition is located at disk0s1
(If it's not, make sure you use the proper location. It should be though.)
![wheres-esp](img/wheres-esp.png)

#### I assume you don't have FileVault2 (full disk encryption) on
(Actually this *should* work with FDE on, but it's more complicated)
![no-fde](img/no-fde.png)

#### I assume your computer is relatively new (2012 or newer)
Your Mac must not have a Core 2 duo or Solo (or older) processor - only the past couple years of Macs, which have 64-bit EFI, are supported.

**SECOND NOTE**: For those who don't know this, a dollar sign ($) indicates that you type the command into a Terminal window. You do not copy the dollar sign. :-)

0. Back your computer up!
0. No really, make sure you have a backup, and make sure to test that it works. This procedure has been tested multiple times, but it's still possible things will go wrong and lose data. On macOS, Time Machine is a great option for backing your computer up.
1. Download rEFInd (http://www.rodsbooks.com/refind/getting.html) as a "binary zip file" and decompress it. Open Terminal and `cd` into the decompressed folder.
2. Install rEFInd to the ESP partition (`$ ./refind-install --alldrivers`)
3. Mount your ESP partition (`$ mkdir /Volumes/ESP && sudo mount -t msdos /dev/disk0s1 /Volumes/ESP/`)
4. Rename the refind directory (`$ mv /Volumes/ESP/EFI/refind /Volumes/ESP/EFI/BOOT`)
5. Rename the refind EFI blob (`$ mv /Volumes/ESP/EFI/BOOT/refind_x64.efi /Volumes/ESP/EFI/BOOT/bootx64.efi`)
6. Fire up Disk Utility and make a new partition/replace your old Linux install partition with a new partition formatted as FAT. Name it something catchy, like "FREYA" (it'll be overwritten in step #14)
7. Plug your USB drive with elementary OS Freya (If you need to make one, check the [Create elementary OS Installer](https://github.com/linusbobcat/create-elementary-os-installer/releases/) or [this](https://github.com/aroman/freya-on-a-mac/tree/master/iso-to-usb) for a more-stable manual guide) into your computer.
8. Pray
9. Reboot and choose the option that indicates that the OS lives on an external/USB disk.
10. Choose to "Try Elementary OS" and, when it boots, search for Terminal
11. Type `$ ubiquity -b` into the Terminal window - when the installer asks about partitioning, make sure you choose **Something Else...**.
12. Find the partiton you created in step #8 and format that as Ext4 and set its mount point to `/`. GRUB will not be installed, since you used the `-b` option for the installer. This setup is completely GRUB free, using rEFInd & EFI-stub loading (the new hotness).
13. Finish installing and restart.
14. Pray
15. Assuming all went well, you should see Ubuntu options in the rEFInd menu. Choose it.
16. Pray
17. You're now dual-booting Freya and OS X. Woot.
18. (optional) Make your rEFInd nicer. You can install a theme, get rid of the duplicate entries, etc. If you want to know how to do that stuff let me know and I'll document it. Here's what my setup looks like:
![no-fde](img/finished-product.jpg)
20. (optional) If you're using a pre-release build for the install, like Beta 1, the WiFi may not work out of the box. In this case you need to install the related drivers manually:
    - *Somehow* get a temporary internet connection, e.g. via your smartphone's USB/Bluetooth tethering function or (*not tested yet!*) a [Thunderbold to RJ45 adapter](http://store.apple.com/us/product/MD463ZM/A/thunderbolt-to-gigabit-ethernet-adapter)
    - execute `sudo apt-get install bcmwl-kernel-source` in a terminal to install the driver and its dependencies
