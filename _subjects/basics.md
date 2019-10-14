---
layout: post
title: "Stuff I use"
---

This post keeps a summary of the various
things I install on my computer.

# App store

+ Slack
+ Outlook

# Sdkman

## languages

```
sdk install kotlin 
sdk install kscript
sdk install java # latest version
sdk install java 8.0.222-amzn # kscript, scala etc need Java 8
```

# Brew

## languages

```
brew install go
brew install crystal
```

## system tools

```
brew install osxfuse # needed to mount plan9 protocol
brew install tree
brew install libgc # needed when compling crystal from source
``` 

## miscellaneous configs

```
git config --global url."git@bitbucket.org:".insteadOf "https://bitbucket.org/"
git config --global url."git@github.com:".insteadOf "https://github.com/"
```

## cloud tools

```
brew install az # azure cli
brew install kubectl # kubernetes.. omg... ponies!!
```

## research and development

```
brew install exercism
```

# Browser

+ Skype for Business (microsoft.com)
+ [Brew](https://brew.sh)
+ [sdkman](https://sdkman.io)
+ Gitter
+ Office 365

# On Machine

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```


# Github

+ [acmecrystal](https://github.com/ilanpillemer/acmecrystal)
+ [plan9port](https://github.com/9fans/plan9port)
+ [Go Fonts](https://go.googlesource.com/image)
+ [9fans Acme Go Tools](https://github.com/9fans/go) # `go get -u 9fans.net/go/...`
+ [acme-lsp](https://github.com/fhs/acme-lsp)
+ go-pls
# Notes on Azure

```
az login
az account set --subscription "subscription you want to use"
az aks get-credentials --resource-group <rg> --name <nm> # sort out tunnels

```

# Slack Groups

+ gameontext
+ londonjavacommunity
+ gophers







