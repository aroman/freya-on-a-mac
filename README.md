### elementary OS on a Mac (using rEFInd & EFI stub loading)

**NOTE**: This guide makes a few assumptions about the your computer you're using. Before we you begin, make sure:

#### You have disabled SIP (System Integrity Protection)
See [here](http://www.rodsbooks.com/refind/sip.html) for more details. In short, it's not hard to do and you can re-enable it as soon as you're finished installing elementary OS.

#### Your Mac is relatively new (2012 or later)
Your Mac must not have a Core 2 duo or Solo (or older) processor. Only the past couple few years of Macs (which have 64-bit EFI) are supported.

#### Your Mac is not a 2015/2016 MacBook or 2016 MacBook Pro
At the time of writing (December 2016), these very new models have limited functionality under elementary OS.

**SECOND NOTE**: For those who don't know this, a dollar sign ($) indicates that you type the command into a Terminal window. You do not copy the dollar sign. :-)

0. Back your computer up!
0. No really, make sure you have a backup, and make sure to test that it works. This procedure has been tested multiple times, but it's still possible things will go wrong and lose data. On macOS, Time Machine is a great option for backing your computer up.
1. Download rEFInd (http://www.rodsbooks.com/refind/getting.html) as a "binary zip file" and decompress it. Open Terminal and `cd` into the decompressed folder.
2. Install rEFInd to the ESP partition (`$ ./refind-install --alldrivers`)
3. Mount your ESP partition (`$ sudo ./mountesp`)
4. Rename the refind directory (`$ mv /Volumes/ESP/EFI/refind /Volumes/ESP/EFI/BOOT`)
5. Rename the refind EFI blob (`$ mv /Volumes/ESP/EFI/BOOT/refind_x64.efi /Volumes/ESP/EFI/BOOT/bootx64.efi`)
6. Open Disk Utility and create a new MS-DOS (FAT) partition. Name it something catchy, like "ELEMENTARY" (it'll be overwritten in step #14)
7. Plug your USB drive with elementary OS (If you need to make one, check the [Create elementary OS Installer](https://github.com/linusbobcat/create-elementary-os-installer/releases/) or [this](https://github.com/aroman/elementary-on-a-mac/tree/master/iso-to-usb) for a more manual guide) into your computer.
8. Pray
9. Reboot and choose the option that indicates that the OS lives on an external/USB disk.
10. Choose to "Try Elementary OS" and, when it boots, search for Terminal
11. Type `$ ubiquity -b` into the Terminal window - when the installer asks about partitioning, make sure you choose **Something Else...**.
12. Find the partiton you created in step #8 and format that as Ext4 and set its mount point to `/`. GRUB will not be installed, since you used the `-b` option for the installer. This setup is completely GRUB free, using rEFInd & EFI-stub loading (the new hotness).
13. Finish installing and restart.
14. Pray
15. Assuming all went well, you should see Ubuntu options in the rEFInd menu. Choose it.
16. Pray
17. You're now dual-booting elementaryOS and macOS. Hooray!
18. (optional) Make your rEFInd nicer. You can install a theme, get rid of the duplicate entries, etc. If you want to know how to do that stuff let me know and I'll document it. Here's what my setup looks like:
![no-fde](img/finished-product.jpg)
20. (optional) If you're using a pre-release build for the install, Wi-Fi might not work out-of-the-box. In this case, you'll need to install the drivers manually:
    - *Somehow* get a temporary internet connection, e.g. via your smartphone's USB or Bluetooth tethering function or (*not tested yet!*) a [Thunderbold to RJ45 adapter](http://store.apple.com/us/product/MD463ZM/A/thunderbolt-to-gigabit-ethernet-adapter)
    - Run `$ sudo apt install bcmwl-kernel-source` to install the driver and its dependencies.
