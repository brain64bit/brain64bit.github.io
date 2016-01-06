---
layout: post
title: "Install Rmagick On OSX Yosemite"
date: 2015-01-08 23:53:22 +0700
comments: true
categories: [ruby] 
---
So i've problem when installing rmagick gem in macosx yosemite, although i've installed imagemagick on my system.
The problem exists when bundler try to install rmagick gem and having trouble finding **MagickWand.h** for building native extension.
```bash
Can't install RMagick 2.13.2. Can't find MagickWand.h.
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of necessary
libraries and/or headers.  Check the mkmf.log file for more details.  You may
need configuration options.
```

<!-- MORE -->

This problem can be solved by tell explicitly to rmagick where the path of imagemagick headers file with providing configuration options on gem installation,
for example like,
```bash
gem install rmagick --with-opt-dir=your_path --with-opt-include=your_path
```
But this problem could be solve easily by installing [pkgconfig](http://en.wikipedia.org/wiki/Pkg-config) first before installing imagemagick. So now you can install pkgconfig first and reinstall the imagemagick.
If you're using **Homebrew**
```bash
brew install pkgconfig
brew uninstall imagemagick && brew install imagemagick
```
Now after reinstallation, you can easilly install rmagick gem as usual **gem install rmagick**
