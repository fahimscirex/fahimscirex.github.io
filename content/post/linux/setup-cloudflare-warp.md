---
title: "Setup Warp on Linux and Get Unlimited Warp Plus Data"
date: 2021-03-28T18:15:05+06:00
draft: false
categories:
- Linux
tags:
- Linux
- WARP
bigimg: [{ src: "/img/cover.webp" }]
---
Cloudflare's Warp is an great service to re-route DNS query and bypass blocked website. Though it has clients for Windows, Mac, Android, IOS it doesn't have a Warp client for Linux. Still we can use an unofficial client to get our work done.


## WGCF

[**WGCF**](https://github.com/ViRb3/wgcf) is an unofficial, cross-platform CLI for Cloudflare Warp. It works with Wireguard. So before installing **WGCF** make sure you've installed

- `wireguard-tools`

- `wireguard-dkms` (if you're using a Linux Kernel older than 5.6)

> Note : You've to restart your system if you install wireguard-dkms.

You can install pre-compiled binary file from it's [releases](https://github.com/ViRb3/wgcf/releases) or you can install it from AUR if you're using Arch derivatives.

## Install binary package
Download the package you need from release page. Then rename the file `wgcf`, make it executable and copy it to `/bin` directory.

```bash
chmod +x wgcf
sudo cp wgcf /bin
```

Or you can install from AUR if you use Arch

```bash
yay -S wgcf
```

Now we will create a Warp account

```bash
wgcf register
```

And generate a Wireguard configuration using that account information

```bash
wgcf generate
```

We've to connect to Warp using that Wireguard configuration. It'd be much easier if you put that configuration file in `/etc/wireguard`

```bash
sudo cp wgcf-profile.conf /etc/wireguard
```

Let's connect now

```bash
wg-quick up wgcf-profile
```

 If you want to disconnect, run

```bash
wg-quick down wgcf-profile
```

Probably you'd like to run Warp every time when your system boots. You can do that with help of systemd

```bash
systemctl enable --now wireguard@wgcf-profile
```


## Troubleshoot

I'm facing some issue frequently. For some unknown reason Wireguard can't connect through my generated configuration profile multiple times. I've found a way out to resolve that. If you face the same issue you've to delete the previous profiles and regenerate new one, except your account profile. To do that you can run this :

```bash
sudo rm -rf wgcf-profile.conf && wgcf generate && sudo rm -rf /etc/wireguard/wgcf-profile.conf && sudo cp wgcf-profile.conf /etc/wireguard
```

Now start the service again. If you want, you can set an alias for this or make a bash script for convenient usage.

## Getting Warp Plus Data for Free

There's a python script from [alilapro](https://github.com/ALIILAPRO/warp-plus-cloudflare) to get free Warp Plus data. We can use that Warp Plus data in Linux also. But we need to get a Device ID to run that. To do that go to `wgcf-account.toml` and copy the `device id`. You can use that ID in alilapro script to get free Warp Plus data.

## Link your Warp account with other device using 1.1.1.1

- Open 1.1.1.1 app. I'm using Android here.

- From hamburger menu go to `Account` > `Key`. Copy the key and save it somewhere else we will need this.

- Go to `Advanced` > `Diagonistics` (optional)

- Under `Client Configuration` you'll see a ID. Copy that and save it. (optional)

Now we've to put our license key into `wgcf-account.toml`. Open the toml file, replace the existing license key with yours and update your configuration profile using `wgcf update` and repeat the previous process.

```bash
wgcf update
sudo cp wgcf-profile.conf /etc/wireguard
```
Now you can use Device ID which you got from toml file or Configuration ID of your 1.1.1.1 in alilapro script.
