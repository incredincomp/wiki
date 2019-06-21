---
description: >-
  If you've updated something, and now your dual booted Debian distro finds
  itself in a kerfuffled situtation.. NVME disk doc
---

# Fixing Grub Boot Issue

So, I had some crit updates to my laptop bios which rendered my Kali disk un-bootable. Windows was being a bully and wouldn't let me do anything, but this is how I fixed it.

### Change bios/uefi settings so that you can boot from USB

Hopefully you have a live Kali USB, and if not you should definitely have one \(or 6.\) Making one of those is beyond the scope of this write up though.  Maybe I will later but I don't tend to write about anything that hasn't happened recently.

Boot into Live Kali mode and open a terminal.  You're going to need to know what your hard disk is named by your system, you either will have `sda-b` or some variant of `nvme0n1`

To find out the names of the things you will need to accomplish this, hit your terminal with a hearty `sudo fdisk -l`

```text
# sudo fdisk -l
Disk /dev/nvme0n1: 477 GiB, 512110190592 bytes, 1000215216 sectors
Disk model: SAMSUNG MZVLB512HAJQ-000L7              
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt

Device              Start        End   Sectors   Size Type
/dev/nvme0n1p2    1024000    1228799    204800   100M EFI System
/dev/nvme0n1p4    1261568  681766911 680505344 324.5G Microsoft basic data
/dev/nvme0n1p5  681766912 1000212479 318445568 151.9G Microsoft basic data
/dev/nvme0n1p6 1000212480 1000214527      2048     1M Linux swap
```

As you can see here at Line 2, my `nvme0n1 = NVMEX` and that is the name of my entire disk.  On lines 10-\* of your own system, you will find all of the information that will help you fix your drive and regain control of your system kernel. 

{% hint style="info" %}
Keep the output from the previous command handy for this next step.
{% endhint %}

### **TAKE CARE WITH WHAT YOU TYPE HERE**

```text
# sudo mount /dev/nvme*** /mnt
# sudo mount /dev/nvme** /mnt/boot/efi
# for i in /dev /dev/pts /proc /sys /run; do sudo mount -B $i /mnt$i; done
# sudo chroot /mnt
  grub-install /dev/nvme*
  update-grub  
```

{% hint style="info" %}
nvme\* = whole disk \| nvme\*\* = efi partition \| nvme\*\*\* = system partition 
{% endhint %}

`quit` - drop yourself out of chroot

`sudo reboot` and you should now be able to access the GRUB menu from your boot load screen. Happy hackin'!

