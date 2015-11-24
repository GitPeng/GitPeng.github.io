---
layout: post
title:  "[watchOS] Develop For watchOS 2 I : The New Architecture"
date:   2015-11-24 20:00:00
categories: tutorial
---
In WWDC 2015, Apple announced watchOS 2 for Apple Watch and new version WatchKit. This is a serial to show you how to work with them. Because the OS and SDK will be updated till they're released late this year, the content in the blog will be changed accordingly.

The unchanged is the seperation of two bundles: Watch app and the WatchKit extension. The Watch app shows the interface and WatchKit extension contains logic codes. There are several new UI elements in WatchKit which we will talk about later.

The main change in architecture is where the WatchKit extension lives. In watchOS 1, the extension is contained in the counterpart iPhone app. While in watchOS 2, the exension is moved to Watch app running  natively in Apple Watch. So the relation between Watch app and iPhone app is changed from kind of "client-server" to peer to peer.

 >![image](https://db.tt/7FpvC5VP)

 <p align="center"> Figure 1-1 Architectures in watchOS 2 and watchOS 1(From [watchOS 2 Transition Guide](https://developer.apple.com/library/prerelease/watchos/documentation/General/Conceptual/AppleWatch2TransitionGuide/index.html)) </p>

This change bring impacts on different use cases. We will talk about the details in articles coming next.

Basically, the impact on starting a new project in Xcode is that you need to select different items when adding watch target.

  ![image](https://db.tt/Mi3GNUs6)

  <p align="center"> Figure 1-2 target for watchOS 1 </p>

  ![image](https://db.tt/qlDpLgJQ)

  <p align="center"> Figure 1-3 target for watchOS 2 </p>

If you want to support both watchOS 1 and watch OS 2, you need to add both targets.

  ![image](https://db.tt/kqyNr33t)

  <p align="center"> Figure 1-4 Code structures </p>

<div id="disqus_thread"></div>
<script type="text/javascript">
        /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
        var disqus_shortname = 'developwatch'; // required: replace example with your forum shortname

        /* * * DON'T EDIT BELOW THIS LINE * * */
        (function() {
            var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
            dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
            (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
        })();
 </script>
 <noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
