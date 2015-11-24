---
layout: post
title:  "[watchOS] Develop For watchOS 2 II: New UI Elements - Alert"
date:   2015-11-24 20:00:00
categories: tutorial
---
In the new WatchKit, we can show alert and action sheet with

    -presentAlertControllerWithTitle:message:preferredStyle:actions:

It's obvious that the title and the message are the messages to the user.

There are three styles: WKAlertControllerStyleAlert, WKAlertControllerStyleSideBySideButtonsAlert and WKAlertControllerStyleActionSheet. As the name indicates, the first and the second are for alert and the last one is for action sheet. Let's see the screenshots for different styles.

In WKAlertControllerStyleAlert style, the buttons are simply arranged vertically as the screenshot shows.

 ![Alert](https://db.tt/YpMk8EAV)

In WKAlertControllerStyleSideBySideButtonsAlert style, the buttons are arranged side by side. The number of buttons are limited up to two.

 ![SSAlert](https://db.tt/KQ8nMwnK)

WKAlertControllerStyleActionSheet is the style for action sheet. The difference is the cancel button is in the top left corner to dismiss the sheet.

 ![ActionSheet](https://db.tt/uMmb7E52)

The infomation like title, style and handler of the button is in WKAlertAction. There are three styles for button : WKAlertActionStyleDefault, WKAlertActionStyleCancel, WKAlertActionStyleDestructive. The default is for most scenarios while cancel is for dismiss the controller without doing anything. The destructive style with red color text is to alert the user about the destructive actions like deleting data.

When one action is selected, the corresponding handler is invoked. The handler is a simple block without parameters and returns.

    typedef void (^WKAlertActionHandler)(void)

Here is an example to show alert controllers in three different styles. In each controller, we will show default, cancel and destructive actions.

<script src="https://gist.github.com/NilStack/c6ed36081b273085f4af.js"></script>

Build and run!

 ![alert](https://db.tt/wd8ySHbh)


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
