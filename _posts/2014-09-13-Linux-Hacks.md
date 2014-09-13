---
layout: post
title:  "Linux Hacks"
date:   2014-09-13 16:00:00
categories: linux
---

### Run a command repeatedly and monitor output: `sensors` for instance

commands line `sensors` outputs a status and exit. We end up running the same command again and again to monitor the status or progress. 

`watch` runs a command repeatedly for us:  
   `watch sensors`  

useful options: 
* `-n 5` - runs every 5 seconds (default 2 secs)
* `-d` - highlight differences

useful to monitor: `sensors`, zfs scrub status


## LVM

### Expand a logical volume, or, What to do when running out of space in one of LVM drives

1. _Extend_ the logical volume. Add enough room.  
   `sudo lvextend -L+1G /dev/mapper/volumegroup-logical`
2. Unmount the volume. Run a disk check.  
   `sudo umount /mnt/mount-point-for-that-logical-volume`  
   `sudo e2fsck -f /dev/mapper/volumegroup-logical`
2. Resize the file system to fit the new room.  
   `sudo resize2fs /dev/mapper/volumegroup-logical`
4. Remount  
   `sudo mount /mnt/mount-point-for-that-logical-volume`

## Troubleshoot

### Suddenly no Window borders, launcher, menus in my Ubuntu

Window manager crashes. Only desktop icons are visible and can be opened. Launcher doesn't show up. Windows appear without borders. Other users' logins are ok.

Proven Workaround: delete _~/.config/dbconf/user

*Warning*: All customizations will be lost -- launcher shorcuts, display tweaks (workspaces, autohide), privacy (search results)

source: http://askubuntu.com/questions/450561/no-window-manager-after-upgrading-13-10-to-14-04

