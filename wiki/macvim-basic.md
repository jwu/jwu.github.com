---
layout: default
title: MacVim Basic
---

# MacVim Basic

## Keyboard Settings

Go To System Preferences → Keyboard → Keyboard, select “Use all F1,F2,….”

## Start From Terminal

```bash
cp mvim /usr/local/bin
```

## Install MacVim with other compile option

```bash
brew install macvim --with-cscope --with-lua --HEAD
```

## Fix vim git commit message error

If you got:

```
error: There was a problem with the editor 'vi'.
Not committing merge; use 'git commit' to complete the merge.
```

Put this command to fix it:

```
git config --global core.editor /usr/bin/vim
```
