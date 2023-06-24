---
title: Stuck at Loading initial ramdisk
date: '2023-06-24'
tags: ['kernel', 'gentoo', 'linux']
draft: false
summary: Yesterday, after I upgraded all of my packages (incl. kernel) and rebooted my laptop, the booting stops and it stuck at "Loading initial ramdisk".
images: []
layout: PostLayout
canonicalUrl: stuck-at-loading-initial-ramdisk
authors: ['default']
---

## Encountering the problem

Yesterday, after I upgraded all of my packages (incl. kernel) and rebooted my laptop, the booting stops and it stuck at "Loading initial ramdisk". The GRUB bootloader is working fine and I can still access Windows 11. Since, it's not GRUB related problem, I suspect that something wrong during the kernel build process. So I booted in livecd (in my case I have Gentoo's minimal installation CD) and chrooted to my Gentoo.

## Temporary solution

The first thing I do is to check the `/boot` directory and it turns out that `/boot` directory is full (the partition capacity is only 100 MiB and since I dual boot my laptop with Windows 11 and Gentoo, it's definitely not enough).

My temporary solution is to delete the old kernel files (like `config-*`, `initramfs-*`, `System.map-*`, and `vmlinuz-*`) to make some space and rebuild the kernel by using this command (I use the distribution kernel, the commands might be different if you use genkernel or manual kernel):

```bash
emerge --ask sys-kernel/gentoo-kernel
```

And... it worked, I can boot to Gentoo again.

## Solution

But, since the boot partition size is still small, whenever I try to upgrade my kernel, the problem might come again. So, the solution is to increase the size of the boot partition (let's say to 512 MiB). On this part, I use KDE Partition Manager to resize my partitions. The boot aka EFI partition (my laptop uses UEFI) is right behind my C:/ partition (Windows). I shrink that partition (move it to the right) to make some space (412 MiB).

Here is another problem, in KDE Partition Manager I can't increase the size of EFI/boot partition. After several times of search in google, I finally found the solution, and the solution is quite easy.

1. Copy/backup all files and directory in the boot partition somewhere else.
2. Delete the boot partition and make a new one with larger size (512 MiB with the same filesystem, don't forget to flag it with the 'boot' flag).
3. Move back all files and directory to the new boot partition.

Don't reboot right now because the GRUB bootloader won't work (if you do, you have to boot liveCD again and chrooted from there). You have to reinstall and reconfigure GRUB. Here is the command I use:

```bash
grub-install --target=x86_64-efi --efi-directory=/boot
grub-mkconfig -o /boot/grub/grub.cfg
```

And now you can safely reboot and use your PC normally again.

## Extra

At a later time, I realized that I can read my package manager (emerge) log, using elogv. And here's what I found:

```
ERROR: postinst
FAILED postinst: 1
Installing the kernel failed

The kernel files were copied to disk successfully but the kernel was not deployed
successfully. Once you resolve the problems, please run the equivalent of the
following command to try again:
```

```bash
emerge --config sys-kernel/gentoo-kernel:6.1.31
```

It turns out that I don't have to rebuild the kernel (it takes time) and just run package specific actions needed to be executed after the emerge process has completed (in this case installing the kernel). Lesson learned, don't forget to read the log first.
