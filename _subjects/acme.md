---
layout: post
title: "Acme and me"
---
After trying several different ways of installing plan9port and acme,
eventually realised that the best way is to fork the repo, pull and
and follow install instructions.
 

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

# Mouse

Get a 3 button mouse. The `2-1` chord is quintessential to really starting to use acme effectively.

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

NB: If you middle-click `Get` in the `+watch` window that gets created it also reruns the
tests. This allows one to re-rerun the tests without having to do no-op edits to run the tests.

### editinacme

    EDITOR=editinacme
    
This will result in various tools using acme as the editor, for example `git commit`.
You need to start the `plumber` so its serving the 9p protocol for this to work.. ie just run `plumber`.

## You also need to add some of your own

These are probably are not the best way of doing these things, I am still learning as I go...
But they work well for me right now.

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

### format comments saved as `cfmt` in `~\bin`

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
     
## The Just Say No

If you use acme, you also have to just say no to the following things as a trade off

* Rectangular Block Selection
* Syntax high-lighting of code
  * Interestingly this has virtually no impact on me once I am focusing
and working
    my mind sees all it needs to see and the syntax highlighting fades
    away. In fact the syntax highlighting perhaps is more distracting
    than you realise, and its more peacock that useful in reality. If
    you need syntax highlighting to understand the code you are
    writing there is probably a bigger problem afoot. Though similar
    to my thoughts about code folding not sure how this applies to
    code you have not written. It is definitely not a problem with Go
    code in the standard library though based on my experience. But
    perhaps code written in languages where syntax needs highlighting
    to be read?


* Code completion
  * The impact this has had on me, is that I now read through the godoc of
each package
    I am working with in the code. As I am restricting my libraries to
    the golang standard library, this means I am reading a lot of high
    quality idiomatic go code, as well as understanding the code in
    more depth, and knowing what it provides. So to my surprise the
    code I am writing is more succint and idiomatic. So this _trade
    off_ is now feeling like a _trade up_.


* Customise Themes
* Code Folding
  * The impact this has had on me is that I am writing code that reads
well
    without code folding. This is positive as it makes the code
    cleaner and simpler to understand. However this still remains a
    problem when reading code not written to read well without code
    folding. Still working on finding a good solution for reasing ugly
    code or files of this nature.



## miscellaneous

[some chatting in comp.os.plan9](https://groups.google.com/forum/#!topic/comp.os.plan9/_YUEVbTFuME%5B1-25%5D)



 