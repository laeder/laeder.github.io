---
layout: post
author: "Mikael Asp Somkane"
title:  "Leaving insert mode without an escape key"
sharing:
    twitter: Leaving insert mode without en escape key
category: [Vim]
tags: [esc]
---

Have you worked on an iPad or other device and suddenly realised that you can't
go back to normal mode because there's no esc key on the keyboard?

## CursorHoldI

Someone mentioned this on Twitter and there was a plethora of answers but I was
thinking there must be an easier way. I remembered the trick I use for [turning
off caps lock][capslock] and there should be something similar in insert mode,
and there is; `` CursorHoldI `` is the same command but in insert mode and to
leave insert mode we have `` stopinsert ``. Since I already use `` updatetime ``
for caps lock I have to store it somewhere while I'm in insert mode and set it
back again when leaving. I set the timeout for insert mode to 5 seconds and
tried it. I entered insert mode, turned on caps lock and waited. After 5 seconds
Vim escaped to normal mode and turned off caps lock. This is the snippet of code
to put in `` .vimrc ``.

``` vim
" automatically leave insert mode after 'updatetime' milliseconds of inaction
au CursorHoldI * stopinsert

" set 'updatetime' to 5 seconds when in insert mode
au InsertEnter * let updaterestore=&updatetime | set updatetime=5000
au InsertLeave * let &updatetime=updaterestore
```

## inoremap

There is also another way and that is to remap a key combination with inoremap
to escape back to normal mode. Use a combination of keys the you're not likely
to use when you write. For me **ÖÖ** works for that. In `` .vimrc `` that would
look like this:

``` vim
inoremap ÖÖ <esc>
```

That means that I can type ÖÖ in insert mode and Vim will escape into normal
mode. I tried that too.

## ctrl+o

There is actually a third way and that is to run `` stopinsert `` in command mode with
ctrl+o. Ctrl+o lets you run one command in command mode and after that Vim goes
back to insert mode. But if you run `` :stopinsert `` Vim will escape into
normal mode.

[capslock]: {% post_url 2019-06-27-turn-off-caps-lock-in-vim %}
