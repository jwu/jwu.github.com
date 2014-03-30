---
layout: default
title: GDB Basic
---

# GDB Basic

## Get the exsdk's strid string 

In XCode, open the debug console, then type print ex_strid_to_cstr(34), the 34 is your expect strid_t.

## Watch a changes variable

In the gdb console. if you do something like:

```bash
(gdb) watch foo->bar->baz
```

gdb will watch *all three locations* so you will know if the result of the expression changes for any reason. 
This is sometimes what you want, and sometimes not. If you know you just want to watch a simple memory location, it is sometimes easier to do:

```bash
(gdb) print  &(foo->bar->baz)
$1 = (int *) 0x100436f10
(gdb) watch *<HEX address returned from the previous command> 
==> watch *0x100436f10
```

The * in this case tells gdb that you are giving it an address, and not an expression... And of course, without the <>'s...
If you want more help on how to use the gdb console, there are docs in /Developer/Documentation/DeveloperTools/gdb/gdb/gdb_toc.html.
