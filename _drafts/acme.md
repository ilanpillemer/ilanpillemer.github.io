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
2. The way homebrew installs it is not aligned with some of the tooling 
   built by others and then things don't work without fidgeting.  
3. If I do want to patch anything, the path is easier. 

```
git clone git@github.com:ilanpillemer/plan9port.git plan9
git remote upstream add git@github.com:9fans/plan9port.git
```

According to the documentation the standard place for plan 9 is

    /usr/local/plan9 

Set up a symbolic link and build in there. I think this is probably
better, just in case someone has hard coded that path somewhere in
some useful utility from the commmunity.

    sudo ln -s /Users/ilanpillemer/git/plan9 /usr/local/plan9
   
# various changes to profile environment
```
PLAN9=/usr/local/plan9 export PLAN9
PATH=$PATH:$PLAN9/bin export PATH
```

# FUSE

Acme presents to other programmes as a file system. And Acme serves the Plan 9 File Protocol. FUSE allows you mount Acme into the kernel file tree, which means you don't need to have special support in any programme you would like to interact with acme.

If you have not mounted the file system with FUSE you can still interact with the acme fileserver via `9p`

    9p ls acme

All the file servers from 9plan are announced as unix sockets in a namespace directory onto 
which [FUSE](https://osxfuse.github.io/) can mount.  

```
brew cask install osxfuse
```

After rebooting and setting necessary permissions for osxfuse.

```
plan9port (master) -> namespace
/tmp/ns.ilanpillemer._private_tmp_com.apple.launchd.RyJOO9IIg7_org.macosforge.xquartz:0
plan9port (master) -> ls `namespace`
acme	font

9pfuse `namespace`/font /mnt/font

Some fonts to try and compare.... [more fonts can be found here..](https://fonts.google.com/)

9 acme -f /mnt/font/Hack-Regular/13a/font -a
9 acme -f /mnt/font/FiraCode-Regular/13a/font -a
9 acme -f /mnt/font/FiraCode-Medium/13a/font -a
9 amme -f /mnt/font/lucsans/typeunicode.7.font


You can also try out different fonts in a window by executing something like the below

    Font /mnt/font/Hack-Regular/13a/font
    
9pfuse `namespace`/acme /mnt/acme

```

If you are fused you can create new windows in acme very easily via the mount
through commands such as

    grep -n ilan * >> /mnt/acme/new/body


[ohnoes](https://github.com/9fans/plan9port/issues/136), so dont mount acme using osxfuse with `-m` switch 
for now... will dig deeper later when I want to use the mount


# Essential tools to be productive

## From the community

The community tools are conveniently available [here](https://github.com/9fans/go)

    go get -u 9fans.net/go/...
    
### acmego

Run this as `acmego -f` from within acme, this will result in `gofmt` running on your `.go` files when you `Put`. :tada:

```
// Acmego watches acme for .go files being written.
// Each time a .go file is written, acmego checks whether the
// import block needs adjustment. If so, it makes the changes
// in the window body but does not write the file. 

   ...<snip>...
   
var gofmt = flag.Bool("f", false, "run gofmt on the entire file after Put")

```
from [here](https://github.com/9fans/go/blob/master/acme/acmego/main.go)

### godef

    go get -u github.com/rogpeppe/godef
    
run as `godef -acme` to jump to definitions. :tada:

### Watch

Run this as `Watch go test` (or similar) in the relevant acme window to enable TDD.

```
// Watch runs a command each time files in the current directory change.
//
// Usage:
//
//	Watch cmd [args...]
//
// Watch opens a new acme window named for the current directory
// with a suffix of /+watch. The window shows the execution of the given
// command. Each time a file in that directory changes, Watch reexecutes
// the command and updates the window.
```
from [here](https://github.com/9fans/go/blob/master/acme/Watch/main.go)

## You also need to add some of your own

### indent region saved as `a+` in `~\bin`

    sed s'/^'/'	'/

NB: Thats a tab, even if you can't see it
 
### unindent region saved as `a-` in `~\bin`

    sed s'/^	'//

NB: Thats a tab, even if you can't see it

### comment region saved as `c+` in `~\bin`

    sed s'/^'/'\/\/'//
    
### uncomment region saved as `c-` in `~\bin`

    sed s@^['	'' ']*//@@ 
    
NB: One or more tabs or spaces....  

### format comments saved as `cmft` in `~\bin`

    c- | fmt -l 60 | c+

NB: See how design philosophy works of building tools on tools here.  

### jump to source code for golang method saved as `def` in `~\bin`

    godef -acme
    
### find all references of saved as `g` in `~\bin`

    grep -n

### open acme saved as `acme` in `~\bin`

```
9 fontsrv &
9pfuse `namespace`/font $HOME/mnt/font
9 acme -f '/Users/ilanpillemer/mnt/font/FiraCode-Medium/15a/font' -a -l acme.dump $*  
```
     
#### notes to self

## go through the following link and mind meld from it

[mind meld](https://groups.google.com/forum/#!topic/comp.os.plan9/_YUEVbTFuME%5B1-25%5D)

### hhmmm
try this font


 