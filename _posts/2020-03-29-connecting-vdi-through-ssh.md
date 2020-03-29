---
layout: post
title: Sharing files across users on the NCI
categories: [nci,vdi]
---

## Description

Sometimes, when using the VDI we just need a terminal window and we don't need to use any of the desktop environment or graphical interface functionality. If this is the case, it's better to connect remotely to the VDI using our terminal and `ssh` -- this is specially convenient if our internet connection is not very good. To be able to connect remotely you'll first need to start a VDI session using Strudel and then you'll have to look at the top of the Desktop window to find the ID of your VDI machine. For example, this is screenshot of a VDI session:

![vdi_screenshot](https://github.com/ANU-WALD/ANU-WALD.github.io/raw/master/files/vdi_screenshot.png)

At the top of the window, you can see there is a code `vdi-n28` which identified this machine. Each time we start a new VDI instance we'll be given a different machine and this number will change, so you'll need to look this up each time.


```
$ ssh pl5189@vdi-n28.nci.org.au
```
