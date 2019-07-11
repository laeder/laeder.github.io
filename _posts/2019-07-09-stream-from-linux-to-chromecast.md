---
layout: post
author: "Mikael Asp Somkane"
title:  "Stream from Linux to Chromecast"
category: [Linux]
---

I was looking forward to getting VLC 3 so that I would be able to stream to
Chromecast, but that didn't work. I remember trying the snap version of VLC 3 in
Linux Mint and that it streamed just fine. But someone in a forum mentioned that
the combination Manjaro and KDE somehow made it impossible. I really like
Manjaro and KDE so I was a bit sad about that, but then I read about Gnomecast
and lo and behold, it streamed!

## Install

You can install Gnomecast by following the instructions on [this site][install].
But if you're using Manjaro you might look for packages without the 3 in pip and
setuptools because it's the default version, to run version 2 though, you have to add
the 2 at the end.

<blockquote>Installerat just means installed in Swedish</blockquote>

### Examples

``` bash
mikael ~ $ python --version
Python 3.7.3

mikael ~ $ python2 --version
Python 2.7.16

mikael ~ $ pacman -Ss python-pip
extra/python-pip 19.0.3-1 [installerat]
    The PyPA recommended tool for installing Python packages

mikael ~ $ pacman -Ss python-setuptools
extra/python-setuptools 1:41.0.1-1 [installerat]
    Easily download, build, install, upgrade, and uninstall Python packages
```

You can read how to use it on Gnomecast's own page on Github at
[keredson/gnomecast][gnomecast].

I had a few problems though.
1. It did show up in my menu, but when I clicked it, KDE couldn't find it. To
   fix that I right clicked the menu icon and chose *Edit application* and in
   the *Application* tab I edited the *Command* from **gnomecast** to
   `` python -m gnomecast ``. This also works for Alt+F2.
2. I also have to quit it and start it again if I want to watch another movie.
   That I have not fixed.
3. It took me a while to see the play buttons, because they're almost invisible. 

Other then that, it just works. It also adds subtitles if you have a file.

[install]: https://www.linuxuprising.com/2018/05/cast-videos-to-chromecast-on-linux-with.html
[gnomecast]: https://github.com/keredson/gnomecast


