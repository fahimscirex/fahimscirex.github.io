---                                                                    
title: "OneDrive Synchronization in Linux"
date: 2021-06-12T22:15:05+06:00
draft: false
categories:
   - Linux
tags:
   - Linux
   - OneDrive
 bigimg: [{ src: "/img/cover.webp" }]
---
OneDrive doesn't have any official client in Linux. So after getting a 5TB OneDrive storage I went for two clients 

- [onedrive-abraunegg](https://github.com/abraunegg/onedrive/): abraunegg's fork of OneDrive CLI client by skilion. Basically it does the synchronization part.

- [onedriver](https://github.com/jstaf/onedriver) : it has a wicked simple GUI. Onedriver will mount your OneDrive storage as a native Linux file system.

## Installation

Let's install them first. I'm going to install onedriver-git, cause it has some extra GUI functions.

```bash
$ yay -S onedrive-abraunegg onedriver-git
```

For other distribution please check those repo.

## Authorization

`onedrive-abraunegg` can be run by just entering `$ onedrive`, it will show a specific link and ask for authorization. I'm going to follow that link, complete authorization, copy the URL of that blank authorization page and finally paste it to the terminal.

For `onedriver` either you can use GUI or terminal. Run `$ onedriver -a` and Login to OneDrive. Mount the OneDrive storage and enjoy. And we're done with onedriver here, rest of the post is about `onedrive` or you can say `onedrive-abraunegg`.

## Setup

We need a config file to use `onedrive` properly, package comes with a config, we've to copy that to user config folder.

```bash
$ mkdir -p ~/.config/onedrive/
$ cp cp /usr/share/doc/onedrive/config ~/.config/onedrive/config
```

You may want to edit the config file, so that anything doesn't go wrong. I'll suggest you to change `sync_dir` to different location and `upload_only` and `no_remote_delete` to true. But if you're willing to store your cloud data to your local system you should keep `upload_only` unchanged.

Now put some files in your sync folder or if you already have data in your cloud simply run `$ onedrive --synchronization` and your sychronization will start. Also you can run `$ onedrive -m` it will monitor changes in sync directory and will sync automatically.

If you change  the  filters  (skip_file or skip_dir in your configs) you must execute `$onedrive --synchronize --resync`. 

## Systemd Intigration

It's convenient to start `onedrive` as a systemd service.

Enable service as root user

```bash
$ systemctl enable --now onedrive
```

Or,

Enable as non-root user (recommended)

```bash
$ systemctl --user enable --now onedrive
```

 To stop the service

```bash
$ systemctl --user disable --now onedrive
```

If you think you need more detailed instruction for usage check the GitHub [repo](https://github.com/abraunegg/onedrive/blob/master/docs/USAGE.md).


