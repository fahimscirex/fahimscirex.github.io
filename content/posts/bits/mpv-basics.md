---
title: "Absolute Basics of MPV"
date: 2023-02-22T18:48:01+06:00
draft: false
categories:
- Techtalk
tags:
- mpv
cover:
    image: "/img/mpv.png"
    alt: basics of mpv
    caption:
    relative: false
description: This short guide on mpv will help you to start with configuring the video player on your own, including mpv.conf and installing mpv-scripts on your linux system.
---

For media players there are plenty of options to choose from. But I like to stick with mpv, mostly because it's absolutely minimal and higly configurable. It doesn't have an UI like VLC or Pot Player, but it's very easy to use and you're not bound to anything, write your scripts and configure as you want. If you don't know scripting, mpv-scripts have got you covered with a huge collection of scripts from the community.

Now, when I said it's minimal I actually meant it. Sure, default config works out of the box but you may want to take it a few step further. That's where this guide is going to help you, at least I hope it would.

## Configuration File
The file *mpv.conf* contains all of the required configuration. The system-wide location for the config file usually is */etc/mpv* and user-specific directory is `~/.config/mpv/`.

Config file looks like this :

```
# Use GPU-accelerated video output by default.
vo=gpu
# Use quotes for text that can contain spaces:
term-status-msg="Time: ${time-pos}"
```

## Video

Here is my current config that handles the video section :

```
vo=gpu-next
gpu-api=vulkan
vo-vaapi-scaling=hq
hwdec=vaapi
hwdec-codecs=all
scale=ewa_lanczos
scale-blur=0.981251
```

### `vo=<driver1,driver2,...[,]>`
By default the selected video output driver is *gpu*, as the name suggests it's gpu-accelarated. It's fairly enough, and that's what you should use if you don't want any hassle. But I use *gpu-next*, which is experimental driver and has bugs. It has almost everything what *gpu* offers, but it's faster, provides better quality. video drivers can be overridden using the *vo* flag

Also there are drivers that are only compatible with X11, such as *xv*, *x11*, *vdpau*. Dedicated *rpi* driver for Raspberry Pi. *dmabuf-wayland* and *wlshm* for Wayland etc. Check [this](https://mpv.io/manual/master/#video-output-drivers) for all available drivers. 

### `gpu-api=<type>`
*gpu-api* controls which type of graphics APIs will be accepted. Available options are *auto*, *opengl*, *vulkan*, *d3d11*. If you have relatively modern hardware you should use *vulkan*, as it gives better performance but if you face issues you should get back to *opengl* that's the default.

### `hwdec=<api>`
Specifies which hardware decoding api should be used, see [this](https://mpv.io/manual/master/#options-hwdec) for available options. Hardware decoding is turned off by default, as it requires modern hardware. If you don't want to go through which options is suitable for you, just use *yes* it will automatically use available decoders. If you explicitly want to specify then use *nvdec* for NVIDIA GPU's otherwise you should use *vaapi* for the most cases.

### `hwdec-codecs=<codec1,codec2,...|all>`
You can specify which codec you would like to use for hardware decoding. The special value *all* always allows all codecs. You can get the list of allowed codecs by running `mpv --vd=help`.

### `scale=<filter>`
It is used to upscale the video and it's only available if you're using gpu, gpu-next or libmpv as video output driver. For available filters check [this](https://mpv.io/manual/master/#options-scale). Upscaling isn't really necessary if the video you're playing matches the resolution of your monitor. By setting *scale-blur=0.981251* imitates *ewa_lanczossharp* filter, which is currently deprecated.

## Audio

My current config that handles audio output :

```
ao=pipewire
volume=100
volume-max=200
```

### `ao=<driver1,driver2,...[,]>`
Usually it isn't necessary to specify audio output drivers, mpv does that fine. Check [this](https://mpv.io/manual/master/#audio-output-drivers-ao) for available audio output drivers. I'm using *pipewire* as this is what I use on my system.

### `volume=<value>`
Specify the default volume that will be set when mpv starts up.

### `volume-max=<100.0-1000.0>`
Set the maximum amplification level in percent (default: 130).

## Language Priority

```
alang=en,eng
slang=en,eng
```

*alang* refers to audio language and *slang* refers to subtitle language you can add as much language as you can, mpv will follow that order.

## Screenshot

```
screenshot-format=png
screenshot-png-compression=1
screenshot-directory="~/Pictures/mpv-screenshots"
screenshot-template="%F - [%P]v%#01n"
```

### `screenshot-format=<type>`
Available choices are *png*, *jpg*, *jpeg*, *webp*, *jxl*.

### `screenshot-png-compression=<0-9>`
Set the PNG compression level. Higher means better compression. Too high compression might occupy enough CPU time to interrupt playback. Check [this](https://mpv.io/manual/master/#screenshot) for other options dedicated to the formats.

### `screenshot-directory=<path>`
Set your desired directory as default for screenshots.

### `screenshot-template=<template>`
Specify the filename template used to save screenshots. The template specifies the filename without file extension. Check [this](https://mpv.io/manual/master/#options-screenshot-template) for further instruction.

`%F - [%P]v%#01n` this template takes video filename, playback time, number of screenshot and puts together in screenshot filename.

Here's how the config file looks after adding all of the section together : 

```
## Video
vo=gpu-next
gpu-api=vulkan
vo-vaapi-scaling=hq
hwdec=vaapi
hwdec-codecs=all
scale=ewa_lanczos
scale-blur=0.981251

## Audio
ao=pipewire
volume=100
volume-max=200

## Language Priority
alang=en,eng
slang=en,eng

## Screenshot
screenshot-format=png
screenshot-png-compression=1
screenshot-directory="~/Pictures/mpv-screenshots"
screenshot-template="%F - [%P]v%#01n"
```

This is the absolute basic config that you can start with.

## User Scripts

mpv has support for scripting. Scripts should be put inside `~/.config/mpv/scripts` directory, mpv will load them while starting up. You'll find a huge collection of scripts created by the community in the [github wiki](https://github.com/mpv-player/mpv/wiki/User-Scripts) of mpv.

### [mpv-script-directory](https://nudin.github.io/mpv-script-directory/)
This webpage indexes various scripts which are searchable and can be filtered to find your desired script easily.

### [mplug](https://github.com/Nudin/mplug)
mplug is a plugin manager for mpv. Basically it helps you to search, install, uninstall mpv scripts and manage them conveniently.
