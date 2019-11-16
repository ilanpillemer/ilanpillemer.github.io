---
layout: post
title: "Stuff I use on Linux"
---

This post keeps a summary of the various
things I install on my computer.

# Distro

+ Ubuntu Live CD

# snaps

+ `sudo apt install git`
+ `sudo snap install emacs --classic`
+ `sudo snap install crystal --classic`
+ `sudo apt-get install gcc pkg-config git tzdata \
                           libpcre3-dev libevent-dev libyaml-dev \
                           libgmp-dev libssl-dev libxml2-dev`
+ `sudo apt install libx11-dev`
+ `sudo apt install apt-file`
+ `sudo apt intall libxt-dev`
+ `sudo apt install libfontconfig1-dev`
+ `sudo apt install libxext-dev`
+ `sudo snap install go --classic`

# Plan 9

```
PLAN9=/home/ilanpillemer/Repos/github/plan9port export PLAN9
PATH=$PATH:$PLAN9/bin export PATH

```

# Pony

+ https://github.com/ponylang/ponyc/blob/master/README.md#installation


```
sudo apt-get install \
     apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common

sudo apt-get install g++

https://github.com/ponylang/ponyup
curl --proto '=https' --tlsv1.2 -sSf https://raw.githubusercontent.com/ponylang/ponyup/master/ponyup-init.sh | sh

export PATH=/home/ilanpillemer/.pony/ponyup/bin:$PATH
```

# Caps to Ctl

`sudo apt install gnome-tweak-tool`