---
layout: post
title:  "[watchOS] Develop For watchOS 2 III : Drawing"
date:   2015-11-24 20:00:00
categories: tutorial
---
There is still no UIView and the UI components in WatchKit don't give us enough customization. But the good news is Core Graphic and UIBizerPath are coming which gives us ability to draw what we want. To present our drawing, we need a WKInterfaceImage because there is no canvas like UIView or CALayer.

If you aren't familiar with Core Graphic and UIBezierPath, please refer to [Quartz 2D Programming Guide](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/Introduction/Introduction.html) and [Drawing Shapes Using Bézier Paths](https://developer.apple.com/library/ios/documentation/2DDrawing/Conceptual/DrawingPrintingiOS/BezierPaths/BezierPaths.html)

Let's get started.

As usual, create iOS app project with a single view for simplicity and add new target for watchOS application.

First, drag the WKInterfaceImage in Object Library to the main Interface in storyboard and connect it to the IBOutlet in your code. We set both the width and height to Relative to Container to make the image cover full watch screen.

 ![image](https://db.tt/JSan1zoH)

Then let's begin to draw a circle and string with simple Core Graphic.

<script src="https://gist.github.com/NilStack/37f2396515d58f8e7592.js"></script>

The result is here.

 ![image](https://db.tt/hlMcXs1D)


Then we will use UIBezierPath to do the same thing.

I open sourced my project [NKWatchChart](https://github.com/NilStack/NKWatchChart) which highly uses drawing.

 ![gif](https://db.tt/d7pJD84m)

<script src="https://gist.github.com/NilStack/a8eb48e899cd4c736562.js"></script>

So it brings the possibility to draw beautiful charts and other things as we need.

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
