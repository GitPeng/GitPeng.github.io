---
layout: post
title:  "[watchOS] Develop For watchOS 2 VIII : Send Message With WatchConnectivity"
date:   2015-12-08 20:00:00
categories: tutorial
---
Before diving into technical details, we will show an intresting game WappyBird built with WatchConnectivity framework. We can fly Flappy Bird with Apple Watch!

 ![WappyBird](https://db.tt/ePQVPKUY)

The complete project is on [GitHub](https://github.com/NilStack/WappyBird).

WatchConnectivity give us different APIs for different scenarios. We will introduce sendMessage:replyHandler:errorHandler: in this article while others will be explained in coming blogs.

Before the real communication starts, WCSession need to be set up in both Watch extension and iOS app. WCSession is a manager who is in charge of the whole process. Usually, the code snippet is like  

Objective-C:

    if ([WCSession isSupported]) {
        WCSession* session = [WCSession defaultSession];
        session.delegate = self;
        [session activateSession];
    }

Swift:

    if WCSession.isSupported() {
        let session = WCSession.defaultSession()
        session.delegate = self
        session.activateSession()
    }

The logic is simple that we first check if the WCSession is supported, then set up default session, set delegate and activate the session.

sendMessage:replyHandler:errorHandler: is used to start immediate communication. Before sendMessage, it is the sender's responsibility to check if the counterpart is reachable with WCSession's read-only property reachable.

Objective-C:

     WCSession *session = [WCSession defaultSession];
     if ([session isReachable]) {
         ...
     }

Swift:

    let session = WCSession.defaultSession()
    if(session.reachable) {
        ...
    }

Both sessions on each side should be reachable which means the Watch app is foreground and the session in iOS app is at least active.

On the receiver side, WCSessionDelegate should be conformed to. In the

    - session:didReceiveMessage:

 or


    - session:didReceiveMessage:replyHandler:

we can handle the message and reply to the sender with the second one.

There is another option if we want to send NSData more than NSDictionary. Please refer to

    - sendMessageData:replyHandler:errorHandler:


In the interesting game we showed at the beginning of the article, please pay attention to Scene.h/m in iOS target and InterfaceController.h/m in Watch Extension if you only want to ignore the game logic.

Enjoy!

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
