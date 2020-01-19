---
title: "Setting Up PyQt & QT Designer on MacOS"
date: 2020-01-16T16:55:12-05:00
draft: false
toc: false
comments: false
categories:
- Software Development
tags:
- Python
---

This article explains how to add some GUI to your Python code.
<!--more-->
Us coders know how to run Python via the Terminal (Command Line if you're a Windows developer), but I highly doubt your nontechnical user audience wants to see anything remotely similar to a terminal. That's where graphical user interfaces (GUIs) come in. And, luckily for Python users, there exists an easy interface to create these GUIs.

Since you're using a Mac, might as well install an amazing (and free) package manager for MacOS, [Homebrew](https://brew.sh/). All you have to do, like the website says, is type `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"` into your terminal and it'll install.

Next you're going to want to install PyQt. In this example, we will be installing PyQt (the latest version). Simply type `brew install pyqt5` and it will install PyQt5 and all of its necessary dependencies. You make have to install or update Xcode. If so, first type `xcode-select --install`, then go [here](https://developer.apple.com/download/more/) and install the latest Xcode version (at the time of writing this, Xcode was on version 11.3). Once that finishes installing, close your terminal if you haven't already and open up a new terminal before typing in `brew install pyqt5` and having it finally run successfully. 

 Now you want to [install Qt Designer](https://build-system.fman.io/qt-designer-download), not Qt Creator, as Qt Creator has a bunch of unnecessary things whereas Qt Designer is only 40 MB.

 If anyone wants me to, I can create another blog post explaining how to create a basic PyQt5 app, but there are numerous sites that already show you how (also, I'd be getting most of my information from them myself as I haven't used PyQt5 in a few years).