---
title: "Set Gtk File Picker"
date: 2021-06-26T11:22:14+06:00
draft: false
categories:
   - Linux
tags:
   - Linux
   - qt5ct
description: Change default file picker of Linux to a GTK based one.
---
I'm using Archcraft with bspwm right now. It uses qt applications as default mostly. I don't like the default qt file picker dialog it uses. And couldn't find any good guideline to change the picker. So I'm going to write down a workaround I got from [@ahmubashshir](https://github.com/ahmubashshir).

We need some dependencies :

- qt5ct
- kvantum-qt5
- xdg-desktop-portal-gtk

Create a config file under `~/qt5ct/qt5ct.conf` with the following content :

```bash
[Appearance]
standard_dialogs=gtk3
style=kvantum-dark
```

Now set the following environment variable at startup.

```bash
exec QT_QPA_PLATFORMTHEME=qt5ct
```

As I'm using bspwm I've put that on bspwmrc. You can put that on `~/.xprofile` or something like that if you're using X11.

That's all you need to do. If you don't know anything about environment variable, I suggest you to [duck](https://duck.com) about that.

