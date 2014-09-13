---
layout: post
title:  "Stuff on top of Windows"
date:   2014-09-13 16:00:00
categories: windows
---


## Basics

* Firefox. Sync
* Chrome. Sync
* MSOffice / LibreOffice
* GVim / Notepad++

## Audio

* Reaper
* Soundcard driver
* Hydrogen Drum machine

## Dev

* Android SDK
* Virtualbox. Extension pack?
* Visual Studio, VsVim, Relative Number
* GitHub for Windows

### Untested

http://processhacker.sourceforge.net/
http://foldersize.sourceforge.net/

## Utilities

* Deluge
* WinCDEmu
* Gimp
* Inkscape
* WinDjView
* Adobe Reader
* Focus Booster
* VirtuaWin (Multiple Workspaces)
* Greenshot (Screenshot)
* evince (better rendering PDF reader and for djvu, etc)
* BatteryCare (for laptop)
* windirstat
* [Wade's Toolbox](http://wademan.com/toolbox)
* [Cmder](https://github.com/bliker/cmder) (based on ConEmu)
* [Pandoc](http://johnmacfarlane.net/pandoc/installing.html) to convert markdown to docx or pdf. Converting markdown to word document is as simple as `pandoc "hello.md" -o "hello.docx"`
* Xmind (mindmapping utility. Mindmaps are kept [here](https://bitbucket.org/rpattabi/stuff).)

## Chess

* ChessMaster
* BabasChess
* Fritz & Stockfish -- Unzip, Fritz Create UCI Engine, Change Main Engine

## Sync

* Google Drive
* Dropbox
* Evernote

## Configuration

### Windows

* Theme (Desktop > Personalize > Browse Themes Online)

#### Disable Automatic Jack Detection (Audio)

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4D36E96C-E325-11CE-BFC1-08002BE10318}\0000\GlobalSettings\EnableDynamicDevices 00 00 00 00

#### How to perform a calibration (full discharge)?

The most adequate method to do a full discharge (100% to a minimum of 3%) consists of the following procedure:

1. Fully charge the battery to its maximum capacity (100%);
1. Let the battery "rest" fully charged for 2 hours or more in order to cool down from the charging process. You may use the computer normally within this period;
1. Unplug the power cord and set the computer to hibernate automatically at 5% as described by the image sequence below (click images to enlarge). If you cannot select 5%, then you should use the minimum value allowed, but never below 5%;
1. Leave the computer discharging, non-stop, until it hibernates itself. You may use the computer normally within this period;
1. When the computer shuts down completely, let it stay in the hibernation state for 5 hours or even more;
1. Plug the computer to the A/C power to perform a full charge non-stop until its maximum capacity (100%). You may use the computer normally within this period.

source: http://batterycare.net/en/guide.html#descTot

### Authentication Proxy

Reference: http://stormpoopersmith.com/2012/03/20/using-applications-behind-a-corporate-proxy/

1. Install [cntlm](http://sourceforge.net/projects/cntlm/files/latest/download?source=files)
1. Update `cntlm.ini` username and domain
1. Important step is to add password hash. Run `cntlm -c cntlm.ini -H` or `cntlm -H -a NTLMv2 -u user@domain proxy:port`
1. Update `cntlm.ini` with the NTLMv2 hash from the previous step.
1. Verify the config with `cntlm -c cntlm.ini -I -M http://www.google.com`.
1. Start the `cntlm` service from windows services.
1. For the application that does not work well with windows authentication proxy, provide proxy and port as 127.0.0.1 and 3128 (this could be changed)
1. The applications should work fine connecting to internet.

### Sync

* Firefox, Chrome, Dropbox

### GVim

<TODO>

### Deluge

* Non-SSD Download folder


## Troubleshoot

### How to speed up MSDN downloads?

To work around this issue, try to download the file in Internet Explorer 9 mode. To do this, follow these steps:

1. In Internet Explorer, open the MSDN Subscriptions download page.
2. In the *Search* box, search for the product that you want to download.
3. Open the Developer Tools pane. To do this, press F12.
4. Open the *Emulation* screen. To do this, press Ctrl+8.
5. On the *Document mode* list under *Mode*, click *9*.
6. On the *User agent string* list under Mode, click *Internet Explorer 9*.

    **Note** After you change the user agent string setting, the download page reloads in Internet Explorer 9 mode.
7. Try again to download the product. The File Transfer Manager application or another download manager should open.
8. After the download begins, close the Developer Tools pane. To do this, press F12 or click the *Close* button in the upper-right corner of the pane.

**Note** After the download begins, you can close the Developer Tools pane without interrupting the download progress. The browser remains in Internet Explorer 9 mode until you close this pane.

