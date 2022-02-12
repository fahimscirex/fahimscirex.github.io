---
title: "Restoring Windows 10 EFI/GPT Bootloader"
date: 2021-06-26T12:40:51+06:00
draft: false
aliases: '/post/journal/restore-windows-10-efi-bootloader/'
categories:
    - Techtalk
tags:
    - Windows
description: Bootloader partition is gone? Can't boot to Windows after installing Linux? Don't worry, follow this guide and restore your bootloader partition.
---
Few days ago I installed Archcraft again. The latest update has some cool stuffs, I wanted to try them. As usual both of my bootloader was gone, I use a dual booted system. I've faced that issue many times and fixed that on my own. But this time it was a litte different, this happened for the first time after switching to UEFI from Legacy. Also it was the first time after I've started using systemd-boot instead of GRUB.

So the thing was my /boot partition got deleted, I don't know exactly how that happened. Cause /boot is a separate partition. AFAIK /boot doesn't get deleted during installation. Probably it was because ABIF, that tool formatted /boot partition. 

As I didn't have backup of my /boot partition, I was panicked that I would've to reinstall Windows to restore the Windows Boot Manager. I spend hours searching for a solution to recreate the Windows 10 bootloader, tried everything I got and finally got a really simple solution.

Let's jump into it.

# What will you need to restore Windows Boot Manager on a GPT/UEFI system?

- ***A pendrive with Windows 10 .iso burned on it.***
  
  I suggest to use the official ISO or something closer to it. Cause customized ISO doesn't work most of the time for this type of work.

- ***An ESP partition.***
  
  EFI System Partitions are in fat32. If you don't have a partition already, create one using diskpart with following commands. If you don't know how to open CMD Prompt then scroll a bit.
  
  ```shell
  > diskpart
  > select disk 0
  > create partition efi size=500 
  > format fs=fat32 quick label=SYSTEM
  ```

- ***And a brain obviously.***

# Restore Windows 10 bootloader

1. Boot a Windows installation medium.

2. Choose your language and click on 'Repair your computer'.

3. Go to Troubleshoot > Advanced Options > Command Prompt.

4. We're going to assign a letter to ESP/Boot partition. Let's assume my ESP volume number is 1.

```shell
> diskpart
> select disk 0
> list volume
> select volume 1
> assign letter f
> exit
```

5. We're done with assiging. Now we will use bcdboot.exe to create a new EFI Windows bootloader. Make sure that c:\Windows is mounted.

```shell
> bcdboot c:\Windows /s f: /l en-us /f ALL
```

Here :

`c:\Windows` - is where Windows is installed.

`/s` - refers to where you want to save the bootloader.

`/l` - language

`/f `- I've set ALL to copy everything. If you don't want to copy everything, just the UEFI stuffs then write UEFI instead of ALL.

Now bootloader should be copied from c: drive to f: drive. Run `bcdedit` to confirm that.

Note : It's better to keep only one ESP. So use bcdboot on a temporary ESP partition and copy files to /boot. So that your systemd-boot stays as it is.

That's all need to do for restoration. Hit me if goes anything wrong. :3

