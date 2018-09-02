---
layout: post
title: "acme and me...em dna emca"
---

# Acme and Me

After trying several different ways of installing plan9port, eventually
realised that the best way is to fork the repo, pull and and follow install instructions.

The reasons are as follows

1. The homebrew version installs a version that is full of bugs and
   you have add a switch to build the latest from head anyway.
   to reach for the mouse. This means being able to patch the code.
2. The way homebrew installs it is not aligned with most of the tooling 
   built by others and then things don't work without fidgeting.  
3. If I want to patch anything, the path is easy. 
   
   
# various changes to profile environment
```
	PLAN9=/Users/ilanpillemer/git/plan9port export PLAN9
	PATH=$PATH:$PLAN9/bin export PATH
```

# FUSE

Acme presents to other programmes as a file system. And Acme serves the Plan 9 File Protocol. FUSE allows you mount Acme into the kernel file tree, which means you don't need to have special support in any programme you would like to interact with acme.

If you have not mounted the file system with FUSE you can still interact with the acme fileserver via `9p`
    9p ls acme

All the file servers from 9plan are announced as unix sockets in a namespace directory onto 
which FUSE can mount.

```
plan9port (master) -> namespace
/tmp/ns.ilanpillemer._private_tmp_com.apple.launchd.RyJOO9IIg7_org.macosforge.xquartz:0
plan9port (master) -> ls `namespace`
acme	font

9pfuse `namespace`/font /mnt/font

Some fonts to try and compare 

9 acme -f /mnt/font/Hack-Regular/13a/font -a -m
9 acme -f /mnt/font/FiraCode-Regular/13a/font -a
9 acme -f /mnt/font/FiraCode-Medium/13a/font -a
9pfuse `namespace`/acme /mnt/acme

[more fonts can be found here..](https://fonts.google.com/)s

```

[osxfuse](https://osxfuse.github.io/) can be installed from a homebrew cask easily.

[ohnoes](https://github.com/9fans/plan9port/issues/136), so dont mount acme using osxfuse with `-m` switch 
for now...

#### notes to self

### Consider

Moving git repo appropriately and installing again at
    /usr/local/plan9 



### hhmmm
try this font

    /usr/lib/plan9/font/lucsans/typeunicode.7.font

