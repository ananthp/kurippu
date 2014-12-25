---
layout: post
title:  "Stuff on top of Ubuntu"
date:   2014-09-13 16:00:00
categories: linux
---


Stuff to install or setup on top of default ubuntu installation

# Launcher

Remove libre office. Add ..  

* Update Manager
* System Monitor
* Terminal

# unnecessary applications

* thunderbird

# MUST HAVE

gimp scribus inkscape vim-gnome git-core curl build-essential keepassx storebackup smplayer wine aptitude lilypond quodlibet scantailor lm-sensors acl terminator soundconverter alsa-tools-gui

acl - Access control lists
alsa-tools-gui - needed for pro sound card mixer
calf-plugins - calf plugins (studio stuff. need to put them in a sep list)

[tomighty](http://www.tomighty.org/download) Pomodoro timer (It doesn't work sometimes.)  
Xmind, Mindmapping tool, from http://www.xmind.net/download/linux/


## games

* gnotravex hex-a-hop gnotski supertuxkart gtans gnect
* puzzle-moppet
* babaschess
* scid stockfish
* [ICOfy](http://sourceforge.net/projects/icofybase/files/ib/scid/) chess games collection

## Firefox

[Favorite addons collection](https://addons.mozilla.org/en-us/firefox/collections/rpattabi/favorites/)

* ad-block plus  
* itsalltext! Also need to apply [this workaround](https://bugs.launchpad.net/ubuntu/+source/firefox/+bug/882125/comments/31)  
* [Dictionary ToolTip](https://addons.mozilla.org/en-us/firefox/addon/dictionary-tooltip/)  
* feedly
* pocket
* clearly (evernote)

# EXPERIMENTAL

scantailor gmusicbrowser anki artha kalvaro krita mono-complete monodevelop  

unity-lens-wikipedia unity-lens-askubuntu  

coffeescript --> nodejs npm & npm install -g coffee-script  

[visualruby](http://visualruby.net)  

ubuntu-restricted-extras <-- not sure now if the option is selected during ubuntu os installation.  

## Firefox
* print edit  

## games
 
* pychess numptyphysics lightsoff bovo pioneers smc palapeli pathological
* circular-chaos  
* super tux kart
* gcompris gcompris-data gcompris-sound-en gnuchess gnucap tuxpaint

### experimental

* pingus, secret maryo chronicles (smc, smc-music), supertux, flightgear flight simulator

# SETUP

* LVM data mountpoints
* File manager bookmarks for lvm mountpoints
* storebackup  ??
* grub theme burg -- [how to](http://www.omgubuntu.co.uk/2010/06/get-animated-themed-icon-only-grub-menu-using-burg-now-simple-to-use)  
* firefox preferences -- content --> don't allow sites to use their own fonts. Looks better.  
* firefox setup sync  
* nautilus - Favourites (/tmp, /mnt/backup2tb)  
* ubuntu one setup  
* keepassx : security - lock database after __ seconds of inactivity  (300 seconds?)  
* transmission blocklist [level1](http://list.iblocklist.com/?list=bt_level1&fileformat=p2p&archiveformat=gz)  
* flash player fix -- http://askubuntu.com/a/131040/1476  <-- didn't require it after ubuntu reinstallation
* storebackup  ??
* vi/vim style for terminal -- update `~/.bashrc` with `set -o vi`
* Music database -- set directories in Quod Libet

### Prompt

Add the following to `~/.bashrc`  
   `PS1="\n\[\033[01;34m\]\w\[\033[00m\]\n \[\033[01;32m\][\W]\[\033[00m\] \$ "`

### Terminator

* Change font to `Latin Modern Mono 10 Regular` size `12`
* Colors: Foreground & Background -> Solarized light, Palette -> Ambience
* Background: Solid Color.

### Caps Lock

In order to disable caps lock:

```
sudo apt-get install gnome-tweak-tool
gnome-tweak-tool &
``` 
Typing > Caps Lock behavior = Disabled / Swap it with Esc key / make it an additional esc

### drive access for all users

references:
* http://askubuntu.com/questions/52584/shared-folders-for-all-users
* http://unix.stackexchange.com/questions/12842/make-all-new-files-in-a-directory-accessible-to-a-group

steps:

  1. edit /etc/fstab. Ensure all the drive/partition has  `/dev/sda1 /mnt/music ext4 defaults,errors=remount-ro,acl 1 2`
  1. unmount to activate acl. `mount -o remount,acl /mnt/music`
  1. create a new user group. `addgroup kutties`
  1. add users to the group. `gpasswd -a ragu kutties` and `gpasswd -a ananth kutties`
  1. add permissions to the group for the existing stuff. `setfacl -Rm g:kutties:rwX /mnt/music`
  1. add permissions to the group for the new files in future. `setfacl -d -Rm g:kutties:rwX /mnt/music`
  1. Ensure remount. `sudo umount /mnt/music` and then `sudo mount /mnt/music`

### Tamil typing support

sudo apt-get install m17n-db m17n-contrib ibus-m17n  
ibus-setup  
choose ibus as the input method in 'System > Language Support'  
in order to have this automatically started on start up `im-switch -s ibus` (??)  
Details [here](http://mmauran.net/blog/?p=102)

### Music collection

* Configure music library paths in Quod libet
* Paned Browser settings: set to _Custom_ and add one by one
   * ~format
   * genre
   * album
   * ~people

### github/git config

```
git config --global user.email "you@example.com"
git config --global user.name "Your Name"

git config --global credential.helper store
```

### vim

* Install vim-gnome
* Install vundle from github. `git clone https://github.com/gmarik/vundle ~/.vim/bundle/vundle`

#### vim bundles

* mattn/gist-vim -- helps to use vim for working with github gists
* unimpaired -- adds easy way to add blank line without going into
insert mode
* fugitive -- do git within vim
* YouCompleteMe
```
sudo apt-get install build-essential cmake python-dev libclang-dev python-clang-3.5
cd ~/.vim/bundle/YouCompleteMe/
./install.sh --clang-completer --system-libclang
```

### settings

Update gvim startup settings with 

```
# ~/.vimrc

colorscheme kellys or colorscheme desert
set guifont=Ubuntu\ Mono\ 12
```

Following was required to be added for ~/.bashrc since gvim took time to start

```
function gvim() { (/usr/bin/gvim -f "$@" &) }
```

#### .vimrc sample

```
set dictionary+=~/.vim/roget13a.txt
set thesaurus+=~/.vim/mthesaur.txt

""
"" Janus setup
""

" Define paths
let g:janus_path = escape(fnamemodify(resolve(expand("<sfile>:p")), ":h"), ' ')
let g:janus_vim_path = escape(fnamemodify(resolve(expand("<sfile>:p" . "vim")), ":h"), ' ')
let g:janus_custom_path = expand("~/.janus")

" Source janus's core
exe 'source ' . g:janus_vim_path . '/core/before/plugin/janus.vim'

" You should note that groups will be processed by Pathogen in reverse
" order they were added.
call janus#add_group("tools")
call janus#add_group("langs")
call janus#add_group("colors")

""
"" Customisations
""

if filereadable(expand("~/.vimrc.before"))
  source ~/.vimrc.before
endif


" Disable plugins prior to loading pathogen
exe 'source ' . g:janus_vim_path . '/core/plugins.vim'

""
"" Pathogen setup
""

" Load all groups, custom dir, and janus core
call janus#load_pathogen()

" .vimrc.after is loaded after the plugins have loaded

colorscheme autumn

" Vundle Stuff
"
set nocompatible
filetype off

set rtp+=~/.vim/bundle/vundle/
call vundle#begin()

Plugin 'gmarik/vundle'
Plugin 'mattn/gist-vim'
Plugin 'canadaduane/VimKata'
Plugin 'valloric/YouCompleteMe'

call vundle#end()

filetype plugin indent on

set guifont=FreeMono\ 11

if has("gui_running")
  " GUI is running or is about to start.
  " Maximize gvim window.
  set lines=999 columns=999
else
  " This is console Vim.
  if exists("+lines")
    set lines=50
  endif
  if exists("+columns")
    set columns=100
  endif
endif
```

## ruby

rvm installation -- http://www.andrehonsberg.com/article/install-rvm-ubuntu-1204-linux-for-ruby-193  
install -- rvm requirements  
vim for ruby -- https://github.com/carlhuda/janus  

```bash
sudo apt-get install build-essential git-core openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake libtool bison subversion pkg-config exuberant-ctags ack

curl -L https://get.rvm.io | bash -s stable --auto #there is --ruby option to install it along with a given ruby
echo '[[ -s "/home/rpattabi/.rvm/scripts/rvm" ]] && source "/home/rpattabi/.rvm/scripts/rvm"' >> ~/.bashrc  # not sure if this is required
source ~/.bashrc  
type rvm | head -1  # this should say rvm is a function  

rvm pkg install openssl  
rvm install 1.9.3 --with-openssl-dir=$HOME/.rvm/usr  
rvm use 1.9.3 --default  
rmdir $rvm_path/usr/ssl/certs
ln -s /etc/ssl/certs $rvm_path/usr/ssl

curl -Lo- https://bit.ly/janus-bootstrap | bash
```
`gem install newgem`

### jruby

reference - https://rvm.io/interpreters/jruby/  

```
rvm requirements # and install the prerequisites for jruby
rvm install jruby --1.9

```

### C++

[vim for c++](http://www.alexeyshmalko.com/2014/using-vim-as-c-cpp-ide/)
vim plugin [YouCompleteMe](http://www.alexeyshmalko.com/2014/youcompleteme-ultimate-autocomplete-plugin-for-vim/)

```
sudo apt-get install clang
```

### Use windows plugins: flash, silverlight, ...

```
sudo add-apt-repository ppa:pipelight/stable
sudo apt-get update
sudo apt-get install --install-recommends pipelight-multi
sudo pipelight-plugin --update

sudo pipelight-plugin --enable flash # or --disable
```

## music visualization

projectm -- `sudo apt-get install projectm-pulseaudio`  
recording visualizations -- http://ubuntuforums.org/showthread.php?t=1497453 and http://www.isep.ipp.pt/roswiki/RecordingOpenGLAppsWithGLC.html  
glc -- https://github.com/nullkey/glc/wiki/Install  
Installing from debian -- add `ftp://ftp.us.debian.org/debian wheezy main` (do not forget to remove after installating)   

## audio conversion

### m4a to flac

```
for f in *.m4a; do avconv -i "$f" "${f%.m4a}.flac"; done  # alternative, replace avconv to ffmpeg
```

## slideshow video

* imagination -- good easy to use software, but lacking good defaults (e.g. ken burns feature is available but should be manually added)
* photofilmstrip -- good defaults. Can render video right away. But lacking many features. But this is good enough for most purposes.

Steps to use photo film strip

1. `sudo aptitude install photofilmstrip`
2. create a new project. Use 16:9 ratio. Don't bother with total time or audio. (Audio file length becomes total time of the video). This gives approx 7 seconds for a picture with ken burn. This is good enough.
3. Add images to the project
4. Selecting an image shows it in right and left panes. With boxes we can customize the ken burn size and direction. Ensure that the left side image is covered fully by the box. Right side image could have the box with the focussed zone of the image or based on the direction of the image. (e.g. mountain images could go from bottom to top i.e. to the sky, macro mode images could ken burned to the focussed point, birds flying images could go to flying direction)
5. It is also possible to change the transition effect and the duration per image.
6. It is also possible to rearrange the images.
7. To get an idea, render (tick icon) in 'draft' mode. The rendering goes on in the job queue tab.
8. Once images, order, ken burn, duration, video quality and format are finalized, give the final render. Preferably HD, PAL, AVI, XVid. This seems to take a lot of time.
9. Since there is no good way to add caption (there is one but it is through srt file), and music (there is an obscure way with very little control), use the output in softwares like kdenlive to get these stuff done.


### cmmi10 font

This one is from latex collection. Used in no-nonsense-calendars. 
http://www.ctan.org/tex-archive/fonts/cm/ps-type1/bakoma/ttf

## Maintenance

### disk check for bad-blocks

`e2fsck -c /dev/fill-me`
`-c` invokes `badblocks` internally. `badblocks` manual recommends this method rather than using badblocks command separately and feeding it to e2fsck -- CAUTION! block size.
Note: ubuntu checks _file systems_ periodically, but doesn't check for badblocks probably. 

For /home, reboot in recovery mode and run `e2fsck`

## Troubleshoot

### Printer prints blank pages till paper runs out

Settings, Printer, make & model -> change
Choose the same Manufacturer, Same model, but try a different foomatic driver shown in the list.
