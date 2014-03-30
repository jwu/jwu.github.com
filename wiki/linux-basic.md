---
layout: default
title: Linux Basic
---

# Linux Basic

## Useful Paths and Files

  * path
    * ~/
    * /etc/init.d/
    * /usr/share/
  * files
    * ~/.gitconfig
    * ~/.vimrc
    * /etc/fstab
    * /var/log/boot.log

## Useful Operations

  - in Desktop Mode, press alt + ctrl + F1 will go to terminal.
  - in Terminal Mode, press alt + F7 will go return to desktop.
  - sudo vim /etc/X11/xorg.conf can modify drivers.
  - /etc/X11/Xsession will start Xsession desktop while in terminal.

## Install Desktop

```bash
# install desktop for ubuntu-server (gnome/kde/xfce)
sudo apt-get install ubuntu-desktop/kubuntu-desktop/xubuntu-desktop
```

## Upgrade

edit file `/etc/update-manager/release-upgrades`. change `Prompt=lts` to `Prompt=normal`, 
save and exit, then execute

```bash
sudo do-release-upgrade -d
```

During the upgrade, it will notify the source address. Press y to continue, 
then wait for it finish upgrade and reboot.

## FAQ

* TODO: how to compile code in different kernel mode ?

## Tools

```bash
# before we install tools, make sure our souce-list in up-to-date.
sudo apt-get update
```

git

```
sudo apt-get install git-all
```

vlc

```bash
sudo apt-get install vlc vlc-plugin-pulse mozilla-plugin-vlc
```

htop

```bash
sudo apt-get install htop
```

## Useful Tools

  - Blender
  - GIMP
  - inkscape
  - mldonkey
  - filezilla
  - goldendict (翻译软件)
  - gnome-do, krunner
  - audacity (声音编辑)
  - vlc (视频播放)
  - clementine (音频播放)
  - pidgin (IM)
