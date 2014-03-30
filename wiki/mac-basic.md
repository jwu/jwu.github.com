---
layout: default
title: Mac Basic
---

# Mac Basic

## Settings

### Keyboard Settings

This settings help macvim use exvim correctly.

Go to System Preferences -> Keyboard -> Keyboard, select "Use all F1,F2,...."

![keyboard_settings_01](../images/keyboard_settings_01.png)

### Bash Profile

reference from: http://hayne.net/MacDev/Notes/unixFAQ.html

When a "login shell" starts up, it reads the file `/etc/profile` and then `~/.bash_profile` 
or `~/.bash_login` or `~/.profile` (whichever one exists - it only reads one of these, 
checking for them in the order mentioned).

## Tips

**capture screenshots on Mac OS X**

  * shift + cmd + 3: fullscreen snapshot.
  * shift + cmd + 4: rect region snapshot.

The picture is saved on the desktop.

**Terminal to Finder, Finder to Terminal**

  * in Terminal, type "open ." will open the Finder and locate in current working directory.
  * in Finder, just drag the folder to Terminal.
    * or you can use http://code.google.com/p/cdto/

**Go in Finder**

In any Finder window or Open/Save dialog, you can hit `Command` + `Shift` + `g` to get a location bar from which you can directly type in the directory to go to.

**Other Tips**

http://apple.stackexchange.com/questions/400/mac-os-x-hidden-features-and-nice-tips-tricks

## Tools

  * compress
    * unrarx: http://www.unrarx.com/
    * KeKa: http://www.kekaosx.com/en/
  * music player
    * cog: http://sourceforge.net/projects/cogosx/
    * vox: http://www.voxapp.uni.cc/
  * installer
    * homebrew: https://github.com/mxcl/homebrew
    * macports: http://www.macports.org/

## Reference

  * Copy Finder Path to Clipboard: http://macdevelopertips.com/applescript/copy-finder-path-to-clipboard-tip-2.html

## Path
  
**Libs and Includes**

the libs/includes are in the `/user/local/include` and `/user/local/lib`
