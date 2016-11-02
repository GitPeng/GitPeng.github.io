---
layout: post
title:  "Swift World: build"
date:   2016-11-02 20:00:00
categories: tutorial
---

This is a tip on how to build Swift on macOS.

1. Prepare system and tools
install latest Xcode
brew install cmake ninja

2. Get source
mkdir swift-source
cd swift-source
git clone https://github.com/apple/swift.git
./swift/utils/update-checkout --clone

3. Build
./swift/utils/build-script -x -R
