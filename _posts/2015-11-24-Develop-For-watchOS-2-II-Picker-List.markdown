---
layout: post
title:  "[watchOS] Develop For watchOS 2 II: New UI Elements - Picker (list)"
date:   2015-11-24 20:00:00
categories: tutorial
---
A new UI elements picker is introduced in WatchKit. Two classes WKInterfacePicker and WKPickerItem are involved. WKInterfacePicker stands for a list of WKPickerItems you can look through and select with Digital Crown.

For different use cases, there are three styles for picker. They are list, stack and sequence. Let's see one by one.

The list style is the simple one. Normally, the UI elements in each WKPickerItem include main title and accessory image. with turning Digital Crown, the list will scroll vertically. Let's see an example.

First, set up iOS app project with a single view and add new target for watchOS application.

 ![image](https://db.tt/AYkEmvyn)

Drag a picker and a label from the Object library at bottom right to the storyboard for watch app.

 ![image](https://db.tt/Ym7kf1ur)

Generate the IBOutlets

 ![image](https://db.tt/LYyQmYaI)

Add the IBAction

<script src="https://gist.github.com/NilStack/85d9afcf23deb245cf0b.js"></script>

Here is the result.

 ![gif](https://db.tt/xBiad8zZ)

In each style, there is two sub-style setting: Focus Style and Indicator. We can set both settings in Attributes Inspector on top left. If set Foucus Style to Outline With Caption, you can get different picker theme.

 ![image](https://db.tt/plEICiAE)

Note: We can't get indicator in simulator even set Indicator in Attributes Inspector. If you get it, please teach me how. Thanks.



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
