---
layout: post
title:  "[watchOS] Develop For watchOS 2 II: New UI Elements - Picker (sequence)"
date:   2015-11-24 20:00:00
categories: tutorial
---
The sequence style "Displays a single image from a sequence of images."(From [WatchKit Framework Reference](https://developer.apple.com/library/prerelease/watchos/documentation/WatchKit/Reference/WKInterfacePicker_class/index.html))

Change the style in Attributes Inspector to Sequence.

 ![image](https://db.tt/jw5Hz54i)

Add a squence of picker items in which contentImage is the corresponding image you want to display.

<script src="https://gist.github.com/NilStack/6be2b6ee143c37c9c327.js"></script>

Build and run!

 ![image](https://db.tt/E6BWcyfC)



Note: ([Radial Bar Chart Generator for Apple Watch](http://hmaidasani.github.io/RadialChartImageGenerator/) is a good tool to generate Apple Watch style radial bar chart sequence images)

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
