---
title: "Fix Your Polybar Panel Icon"
date: 2021-05-23T01:53:19+06:00
draft: false
categories:
- Linux
tags:
- Linux
- Polybar
bigimg: [{ src: "/img/cover.webp" }]
---

[Polybar](https://github.com/polybar/polybar) let's you choose multiple fonts and sizes for your WM panel. So if you're having issues with the size of icons/texts in your panel you can change the size individually. Let's get to the point. How are you supposed to do that?

You've to define which font you want to use and how you want to customize it's value. So put those values in your Polybar config under `[bar/example]`. Here's an example config :

```
[bar/example]
; font-N = <fontconfig pattern>;<vertical offset>
font-0 = "Tamsyn:pixelsize=12;0"
font-1 = "Roboto:size=11:weight=bold;2"
font-2 = "Noto Sans:size=11;1"
font-3 = "Iosevka Nerd Font:size=18;3"
```

If you're wondering what the value after semicolon does. It is used to move the font to the up or to the down.

Let's assume I'm having problem with a Spotify module icon in my panel. That icon is way big than others. So i'm gonna fix that by changing the icon font size. Usually [Nerd Fonts](nerdfonts.com) is used in Polybar to get a icon, so I'm gonna choose Iosevka Nerd Font. As you can see I've added that in the config already.

That Spotify module config looks like this :

```
[module/spotify-icon]
inherit = module/apps
content = 
content-font = 3
click-left = env LD_PRELOAD=/usr/lib/spotify-adblock.so spotify %U &
```

All you've to do is, add the `content-font` value. Here 3 directs to `font-3`.

That's all! I'm linking the Polybar wiki [here](https://github.com/polybar/polybar/wiki/Fonts), just in case you might need it. :D
