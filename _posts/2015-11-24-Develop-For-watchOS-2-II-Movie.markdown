---
layout: post
title:  "[watchOS] Develop For watchOS 2 II: New UI Elements - Movie"
date:   2015-11-24 20:00:00
categories: tutorial
---
WKInterfaceMovie is a new UI components in WatchKit to play video and audio.

Let's get started to build a app to play local video.

As usual, create iOS app project with a single view for simplicity and add new target for watchOS application.

First, drag the WKInterfaceMovie in Object Library to the main Interface  in storyboard and connect it to the IBOutlet in your code.

 ![image](https://db.tt/h1VdTHum)

Now, we need to feed the component with movie by setMovieURL:. Please note WKInterfaceMovie only support local file url which means the media file is in your Apple Watch. In project, place it in Extension for watch app.

Then we can customize the movie with video gravity, poster image and loops.

The video gravity means resizing options including WKVideoGravityResizeAspect, WKVideoGravityResizeAspectFill, WKVideoGravityResize. We can find detailed explanation from  [WKVideoGravity](https://developer.apple.com/library/prerelease/watchos/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/c/tdef/WKVideoGravity).

The poster image is shown to tell the user content hint about the video. Set the loops YES or NO to tell if you want to play the video again and again. We can set these values in Attributes Inspector or programmatically.
<script src="https://gist.github.com/NilStack/aa42f6bcf3c9f563e40e.js"></script>

Build, run and pray!

{<1>}![gif](https://db.tt/FnVvsRJp)


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
