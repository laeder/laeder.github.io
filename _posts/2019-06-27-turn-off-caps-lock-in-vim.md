---
layout: post
author: "Mikael Asp Somkane"
title:  "Turn off caps lock when going back to normal in Vim"
category: [Vim]
---

Ever forgot to turn off caps lock when you escaped back to normal mode in Vim?
Well, yeah, me too. Here's how to fix that.

## In Windows

In Windows it's actually pretty easy, there's a plugin that turns off caps lock
as soon as you escape back to normal. It's available on GitHub at 
[suxpert/vimcaps][vimcaps]

## In Linux

At least for me the plugin didn't work in Linux, but I found another solution
[here][turnoffcaps] and below is the code you put it your `` .vimrc ``.

``` vim
au CursorHold * call TurnOffCaps()
set updatetime=10

function TurnOffCaps()  
    let capsState = matchstr(system('xset -q'), '00: Caps Lock:\s\+\zs\(on\|off\)\ze')
    if capsState == 'on'
        silent! execute ':!xdotool key Caps_Lock'
    endif
endfunction
```

CursorHold is an event trigger and it triggers when you're not typing in normal
mode. The amount of time it's waiting before it triggers is updatetime and it
should be as small as possible. This way as soon as you enter normal mode and
don't type for 10 milliseconds it will call TurnOffCaps.

TurnOffCaps will use xset to check the status of caps lock and if it's on it
will simulate a keypress of caps lock with xdotool to turn it off. And if I
remember correctly I had to install xdotool.


[vimcaps]: https://github.com/suxpert/vimcaps
[turnoffcaps]: https://vi.stackexchange.com/questions/376/can-vim-automatically-turn-off-capslock-when-returning-to-normal-mode

