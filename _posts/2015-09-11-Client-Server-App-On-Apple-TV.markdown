---
layout: post
title:  "Develop Client-Server App for Apple TV"
date:   2015-09-11 21:32:28
categories: tutorial
---
In iOS, it's not easy for our developers to update something like the UI dynamically. We build new version and have to dive into the tedious review process of Apple app store. So there is hybrid app which leverage the web technical stack and web view component.

In tvOS, Apple introduced its own hybrid solution with client-server app. With the new TVMLKit framework, the native tvOS app will access a main Javascript file on our own server and display the pages in Television Markup Language (TVML). The architecture is shown below.

> ![image](https://db.tt/DxAi2umr)
> From App Programming Guide for tvOS     

Let's get started to build a simple client-server app for Apple TV.

On the native side, create a Single View Application for tvOS.

 ![new-project-template](https://db.tt/9WH6auYi)

Then let's do some house cleaning. Please remove view controller header and implementation files, the main storyborad file, and the entry for 'Main storyborad file base name'. Why? Because our UI and logic are all going to the remote Javascript and TVML pages.

It's time to start building our native shell.

<script src="https://gist.github.com/NilStack/fc55a115d86cb8c82416.js"></script>

As the web content side, we need a web server to host our javascript files and pages. We use dropbox public folder to avoid setting up our own.

A simple local server in Node.js or python is also enough for this example. SimpleHTTPServer is good option which is started by command and replace port with real value you want.

    python -m SimpleHTTPServer [port]

If you get error "App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure.". The solution is to set Allow Arbitrary Loads (NSAllowsArbitraryLoads)  of App Transport Security Settings (NSAppTransportSecurity) to YES in the Info.plist.

In the main Javascript file, set up a XMLHttpRequest to get TVML file and push the result to foreground after loading.

<script src="https://gist.github.com/NilStack/7c9121ec373078526d69.js"></script>

In the TVML page, we simply show an alert.

<script src="https://gist.github.com/NilStack/fd56b9385e04014f9b39.js"></script>

Please refer to [TVJS Framework Reference](https://developer.apple.com/library/prerelease/tvos/documentation/TVMLJS/Reference/TVJSFrameworkReference/index.html) and [Apple TV Markup Language Reference](https://developer.apple.com/library/prerelease/tvos/documentation/LanguagesUtilities/Conceptual/ATV_Template_Guide/index.html).

Build and be patient.

 ![Client-Server-Screenshot](https://db.tt/sUgOZ8WC)

The complete project is on [GitHub](https://github.com/NilStack/AppleTVClientServerApp).
