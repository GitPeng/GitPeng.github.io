---
layout: post
title:  "Hello Apple TV!"
date:   2015-09-10 21:32:28
categories: tutorial
---
Apple TV is becoming another platform for our developers while Apple announced tvOS and new generation TV device.

By convention in developers' world, we will develop a 'Hello Apple TV!' style example.

First please download and install Xcode 7.1 beta from Apple developer page. In the templates for new project, you'll see a new catagory for tvOS. Please select the simple single view application template.

 ![new-project-template](https://db.tt/9WH6auYi)

The following steps are same as creating a new iOS applicaton. You will also be very familiar with the generated files.

 ![new-project-files](https://db.tt/YFH8vFvW)

Ok, in the ViewController implementation file, we will add the UI elements and logic. It's very simple to show 'Hello TV!' and the tap gesture on play/pause button of Apple TV Remote controller will change the color of the text.

<script src="https://gist.github.com/NilStack/6d88bb470551f2462f42.js"></script>

We don't use button as usual but a tap gesture because the interacton is very different with iOS even the codes are similiar. We will dig deeper on this topic later.

Build and pray!

 ![HelloTV.gif](https://db.tt/GXGS6Y03)

The complete project is on [GitHub](https://github.com/NilStack/HelloAppleTV).
