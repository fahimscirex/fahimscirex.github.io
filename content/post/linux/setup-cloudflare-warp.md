---
title: "Setup Warp on Linux and Get Unlimited Warp Plus Data"
date: 2021-03-28T18:15:05+06:00
draft: false
categories:
- Linux
tags:
- Linux
- WARP
bigimg: [{ src: "/img/cover.png" }]
---
Cloudflare's Warp is a great service to re-route DNS query and bypass blocked website. Though it has clients for Windows, Mac, Android, IOS it doesn't have a Warp client for Linux. Still we can use an unofficial client to get our work done.


## WGCF

[**WGCF**](https://github.com/ViRb3/wgcf) is a unofficial, cross-platform CLI for Cloudflare Warp. It works with Wireguard. So before installing **WGCF** make sure you've installed

- `wireguard-tools`

- `wireguard-dkms` (if you're using a Linux Kernel older than 5.6)

> Note : You've to restart your system if you install wireguard-dkms.



Now we will create a Warp account

```Code
wgcf register
```

And generate a Wireguard configuration using that account information

```Code
wgcf generate
```

We've to connect to Warp using that Wireguard configuration. It'd be much easier if you put that configuration file in `/etc/wireguard`

```Code
sudo cp wgcf-profile.conf /etc/wireguard
```

Let's connect now

```Code
wg-quick up wgcf-profile
```

 If you want to disconnect, run

```Code
wg-quick down wgcf-profile
```

Probably you'd like to run Warp every time when your system boots. You can do that with help of systemd

```Code
systemctl enable --now wireguard@wgcf-profile
```


## Troubleshoot

I'm facing some issue frequently. For some unknown reason Wireguard can't connect through my generated configuration profile multiple times. I've found a way out to resolve that. If you face the same issue you've to delete the previous profiles and regenerate new one, except your account profile. To do that you can run this :

```Code
rm -rf wgcf-profile.conf && wgcf generate && sudo rm -rf /etc/wireguard/wgcf-profile.conf && sudo cp wgcf-profile.conf /etc/wireguard && wg-quick up wgcf-profile
```

If you want, you can set an alias for this or make a bash script for convenient usage.

## Getting Warp Plus Data for Free

There's a python script from [alilapro](https://github.com/ALIILAPRO/warp-plus-cloudflare) to get free Warp Plus data. We can use that Warp Plus data in Linux also. But we need to get a configuration ID to run that. To do that

- Open 1.1.1.1 app. I'm using Android here.

- From hamburger menu go to `Account` > `Key`. Copy the key and save it somewhere else we will need this.

- Go to `Advanced` > `Diagonistics`

- Under `Client Configuration` you'll see a ID. Copy that and save it.

Now we've to put our license key into `wgcf-account.toml`. Open the toml file, replace the existing license key with yours and update your configuration profile using `wgcf update` and repeat the previous process.

```Code
wgcf update
sudo cp wgcf-profile.conf /etc/wireguard
```

Now you can use that [Python script](https://github.com/ALIILAPRO/warp-plus-cloudflare/blob/master/wp-plus.py) to get free Warp Plus data. During running that script use the ID you've copied before.
