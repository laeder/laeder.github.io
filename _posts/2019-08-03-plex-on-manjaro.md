---
layout: post
author: "Mikael Asp Somkane"
title: Plex in Manjaro
sharing:
    twitter: Plex in Manjaro
category: [Linux]
tags: [plex, gnomecast, chromecast]
---

I know I sang the praise of Gnomecast, but it is way too unstable for me. So I
looked for another solution. I tried Plex some time ago in Linux Mint, but I
didn't get it to work. But this time I did.

I followed [these instructions][installplex] to install Plex and opened the
dashboard in Firefox. And as before, there was an error. I read through a bunch
of forum posts and tried to get something useful from the logs and I almost gave
up when someone mentioned that Plex doesn't work well with Firefox. They said
Edge works but since I'm on a Linux box that is not an option. With low hopes I
opened the dashboard in Chromium and this time it worked! 

Next I tried to connect to the server from my phone and it didn't work at first
until I figured out that I had to log in to the same account at plex in the
phone as the server. That was not necessary before, but it's not a big deal.

[installplex]: https://snapcraft.io/install/plexmediaserver/manjaro


