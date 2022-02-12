---
title: "How did I migrate my user data to another ROM?"
date: 2021-11-11T16:49:25+06:00
draft: false
aliases: '/post/journal/migrate-data-to-new-rom/'
categories:
- Techtalk
tags:
- Android
- Migration
cover:
  image: "/img/migrator.webp"
  alt: "Migrator"
  caption: "Migrate your Android app data"
  relative: false
description: Want to change your ROM but worried about your precious data? Here is an absolute solution to backup your android apps including data.
---

While I couldn't get the Migrate app working in my device I found something that worked for me. [Migrator](https://github.com/VR-25/migrator) : A backup solution and data migration utility for Android.

Basically it's a collection of some shell scripts that will assist you to copy app, data, permissions and other necessary stuffs and restore them afterwards. It's usable as a Magisk module or as a standalone shell script.

- Before changing my ROM I did a backup of user apps by running `M -beM`. This will copy necessary files to `/data/migrator/`. So enough internal storage is needed to do a backup.

- To avoid storage issue or to use the backup in another device you may export it to external sd card by running `M -e -d /storage/XXXX-XXXX`. 
  > Note : XXXX-XXXX is a bunch of characters that represents your SD Card.

- Change your ROM or device, delete partitions or reset the device. Install Migrator module through Magisk or just run the script and import the exported backup by running `M -i -d /storage/XXXX-XXXX/migrator_exported`. If you didn't exported backups, importing is not needed.

- Finally run `M -re` to restore everything. 

> Note : You'll get more information in the README of Migrator's repo, so follow that.
