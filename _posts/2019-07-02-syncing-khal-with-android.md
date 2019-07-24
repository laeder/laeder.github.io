---
layout: post
author: "Mikael Asp Somkane"
title:  "Syncing Khal with Android"
category: [Linux]
tags: [khal, radicale, opensync, vdirsyncer, android]
---

[Earlier][locale] I promised to show you how to sync Khal with Android and here
is how I did it.

## Khal

[Khal][khal] is terminal based calendar and it can also manage birthdays through contact
cards.

This is how I set it up. I have my calendar, one for my daughter and one
for birthdays.

This is my config file for Khal:

###### config

``` conf
[calendars]

    [[mikael]]
    path = /home/mikael/.local/share/khal/calendars/mikael
    type = calendar
    color = light green

    [[alice]]
    path = /home/mikael/.local/share/khal/calendars/alice
    type = calendar
    color = light magenta

    [[birthdays]]
    path = /home/mikael/.local/share/khal/contacts/birthdays
    type = birthdays
    readonly = True
    color = light cyan

[default]
default_calendar = "mikael"
highlight_event_days = True
timedelta = 5d

[locale]
timeformat = %H:%M
dateformat = %Y-%m-%d
longdateformat = %Y-%m-%d
datetimeformat = %Y-%m-%d %H:%M
longdatetimeformat = %Y-%m-%d %H:%M
weeknumbers = right
```

In lines 4, 9 and 14 you can see that all calendars are in
`` /home/mikael/.local/share/khal `` but the contacts are in 
`` /contacts/ `` while the normal calendars are in `` /calendars/ ``. Those are
the directories that vdirsyncer will use. But we will come to that.

In line 20 I set the dafault calender to mine because that's the one I usually
use, while my daughters is for appointments or birthday parties and so on for
her.

And in line 16 the birthday calendar is set to readonly because you add cards to
that directory by hand, not with Khal itself.

A card looks like this:

###### mikael_asp_somkane.vcf

``` conf
BEGIN:VCARD
VERSION:3.0
FN;CHARSET=UTF-8:Mikael Asp Somkane
N;CHARSET=UTF-8:Asp Somkane;Mikael;;;
GENDER:M
BDAY:19690105
REV:2019-05-21T09:37:38.754Z
END:VCARD
```

I made a card for each birthday and named the files after their names. With
every card I change the name, gender and birthday. They will show up when you
run Khal and they will sync and show up in Android.

## Radicale

[Radicale][radicale] is a free and open-source CalDAV and CardDAV server and
that's what I'm using to store the data to and from Khal and Android. Since I
don't run it outside of my network I use a very simple setup with clear text
password.

Config files goes in `` ~/.config/radicale ``.

###### config

``` ini
[auth]
type = htpasswd
htpasswd_filename = ~/.config/radicale/users
# encryption method used in the htpasswd file
htpasswd_encryption = plain

[server]
hosts = 0.0.0.0:5232

[storage]
filesystem_folder = ~/.var/lib/radicale/collections
```

Make sure to create the directory `` ~/.var/lib/radicale/collections ``.

`` users `` is a file with name password pairs. Change it to your name and a
password of your choice.

###### users

``` conf
mikael:*********
```

### Run Radicale as a service

I run Radicale with systemd as a user because it was simpler.

Create the file `` ~/.config/systemd/user/radicale.service ``.

###### radicale.service

``` ini
[Unit]
Description=A simple CalDAV (calendar) and CardDAV (contact) server

[Service]
ExecStart=/usr/bin/env python3 -m radicale
Restart=on-failure

[Install]
WantedBy=default.target
```

Then run these commands:

``` bash
# Enable the service
$ systemctl --user enable radicale
# Start the service
$ systemctl --user start radicale
# Check the status of the service
$ systemctl --user status radicale
# View all log messages
$ journalctl --user --unit radicale.service
```

## Syncing Khal with Radicale

To sync Khal with Radicale you need to use Vdirsyncer and just like Radicale you
set up a config file in `` ~/.config/vdirsyncer/ ``.

###### config

``` ini
[general]
# A folder where vdirsyncer can store some metadata about each pair.
status_path = "~/.vdirsyncer/status/"

# CARDDAV
[pair mikael_contacts]
# A `[pair <name>]` block defines two storages `a` and `b` that should be
# synchronized. The definition of these storages follows in `[storage <name>]`
# blocks. This is similar to accounts in OfflineIMAP.
a = "mikael_contacts_local"
b = "mikael_contacts_remote"

# Synchronize all collections that can be found.
# You need to run `vdirsyncer discover` if new calendars/addressbooks are added
# on the server.

collections = ["from a", "from b"]

# Synchronize the "display name" property into a local file (~/.contacts/displayname).
metadata = ["displayname"]

# To resolve a conflict the following values are possible:
#   `null` - abort when collisions occur (default)
#   `"a wins"` - assume a's items to be more up-to-date
#   `"b wins"` - assume b's items to be more up-to-date
#conflict_resolution = null

[storage mikael_contacts_local]
# A storage references actual data on a remote server or on the local disk.
# Similar to repositories in OfflineIMAP.
type = "filesystem"
path = "~/.local/share/khal/contacts"
fileext = ".vcf"

[storage mikael_contacts_remote]
type = "carddav"
url = "http://localhost:5232"
username = "mikael"
password = "********"

# CALDAV
[pair mikael_calendar]
a = "mikael_calendar_local"
b = "mikael_calendar_remote"
collections = ["from a", "from b"]

# Calendars also have a color property
metadata = ["displayname", "color"]

[storage mikael_calendar_local]
type = "filesystem"
path = "~/.local/share/khal/calendars"
fileext = ".ics"

[storage mikael_calendar_remote]
type = "caldav"
url = "http://localhost:5232"
username = "mikael"
password = "********"
```

As stated on line 14 you have to run `` vidirsyncer discover `` to create
contacts and calendars in Radicale before you can start syncing them.

Once that is done you run `` vdirsyncer sync `` to sync Khal with Radicale.

And when you have done that you can log into Radicale with the user account you
wrote in the users file and change the color of each calendar. These colors with
sync with Android so it's clear what calendar is shown.

## Syncing Radicale with Android

I use [OpenSync][opensync] to do that. Just follow the instructions when you run
it and you'll be fine. If you only sync at home you can tell OpenSync only to
sync when you are at a certain SSID to make sure it can reach Radicale on your
computer. You can also sync manually at any time if you just added something and
want to make sure you get it before you head out.

[locale]: {% post_url 2019-06-28-wrong-default-locale %}
[khal]: https://github.com/pimutils/khal
[radicale]: https://radicale.org/
[opensync]: https://play.google.com/store/apps/details?id=com.deependhulla.opensync&hl=en
