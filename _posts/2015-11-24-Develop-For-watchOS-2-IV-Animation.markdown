---
layout: post
title:  "[watchOS] Develop For watchOS 2 IV : Animation"
date:   2015-11-24 20:00:00
categories: tutorial
---
In first version WatchKit, animation only means an image sequence. Now, real property animation comes true even though there are still not so many properties involved and not so flexible.

Let's get started.

As usual, create iOS app project with a single view for simplicity and add new target for watchOS application.

First, drag a WKInterfaceImage and three WKInterfaceButtons to the main Interface in storyboard and connect them to the IBOutlets in your code. The layout of these components are shown below. We set fixed width and height for image to get the values easily while animating the size.

 ![image](https://db.tt/WDwpqXIb)

The new method in WKInterfaceController is shown below.

    - (void)animateWithDuration:(NSTimeInterval)duration
                     animations:(void (^ _Nonnull)(void))animations

What we only need to do is to set duration and put the code snippet to set values for the animatable properties in the block. The animatable properties are alpha, width/height, horizontal/vertical alignment, background/tint color and group content insets.

But there is no completion block. What if we want to do something right after the animation is completed? We can see a workaround in the codes below to use dispatch_after.  

<script src="https://gist.github.com/NilStack/5f3f916ffcd495ef1986.js"></script>


Build and run.

 ![gif](https://db.tt/jCGmz872)

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
