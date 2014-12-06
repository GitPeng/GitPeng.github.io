---
layout: post
title: iWatch App Development Basic 4 Glance
---

---
<!-- Begin MailChimp Signup Form -->
<link href="//cdn-images.mailchimp.com/embedcode/slim-081711.css" rel="stylesheet" type="text/css">
<style type="text/css">
	#mc_embed_signup{background:#bbdefb; clear:left; font:18px Helvetica,Arial,sans-serif;  width:300px;}
	
</style>
<div id="mc_embed_signup">
<form action="//github.us9.list-manage.com/subscribe/post?u=ff5dae3ddc1f4cead9b9d7277&amp;id=868c3a1b23" method="post" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" class="validate" target="_blank" novalidate>
    <div id="mc_embed_signup_scroll">
	<label for="mce-EMAIL"><h3>Subscribe to Watch App Dev Weekly</h3></label>
	<input type="email" value="" name="EMAIL" class="email" id="mce-EMAIL" placeholder="email address" required>
    <div style="position: absolute; left: -5000px;"><input type="text" name="b_ff5dae3ddc1f4cead9b9d7277_868c3a1b23" tabindex="-1" value=""></div>
    <div class="clear"><input type="submit" value="Subscribe" name="subscribe" id="mc-embedded-subscribe" class="button"></div>
    </div>
</form>
</div>

<!--End mc_embed_signup-->

---

One of the roles iWatch plays is the viewport for information from iPhone. Glance is the direct proof of this role. Here Glance means glance. So it's quick, not detailed and not interactive. The first rule is to focus on the most important information.  

Swiping up, paging through and tapping are the procedure users access Glance. The only action you can respond is the last one. When users tap the Glance, your app will be launched. You should respond with this context.

Let's start build a glance.

As usual, please create new containing iOS project and add watch app target. The difference comes at new target dialog.

 ![image](http://nilstack.github.io/public/image/add_glance_target.png)

Please make sure to check the "Include Glance Scene" checkbox.

![image](http://nilstack.github.io/public/image/glance_main_points.png)

In the storyboard, the Glance Interface Controller Scene are automatically generated. The Glance Interface Controller has two default groups. It has its own Glance Entry Point and does not depend on other Interface Controller Scene. 

There are three default parts: the upper, the lower and the page indicator dots at the bottom. The templates for the two parts are pre-defined and you can only customize your content according to these styles.  That's why Apple call it template-based in HIG. You can't make change to the indicator. 

![image](http://nilstack.github.io/public/image/glance_template_upper_group.png)

![image](http://nilstack.github.io/public/image/glance_template_lower_group.png)

Then let's build Glance for a weather app. First we need to select the right template. For the upper part,  we keep it as a group and put a WKInterfaceImage in it to show the different weather icons.

For the lower part, we need a body to show the temperature and a label to show the location. So i select the Group-Body template and put a label in the big group in the center of the screen.  Set the content of the image and label in Attibute instpector at the top right.

![image](http://nilstack.github.io/public/image/weather-glance-raw.png)

Then here come the different steps. Because the iWatch simulator doesn't support swipping up gesture. The next step of building  needs a little patience. We need to set up a new scheme for our build. If you aren't familiar with build scheme, please go to [Xcode Shceme](https://developer.apple.com/library/ios/featuredarticles/XcodeConcepts/Concept-Schemes.html) in developer document. 

Please select watch app scheme, then Edit Scheme.

![image](http://nilstack.github.io/public/image/edit-scheme-menu.png)

Then duplicate the scheme and give it a name.

![image](http://nilstack.github.io/public/image/duplicate-scheme.png)

Select Run in the left column and your customized name in the Executable of Info tab.

![image](http://nilstack.github.io/public/image/select-glance-executable.png)

Select the Glance target you named in the menu and run.

 ![image](http://nilstack.github.io/public/image/glance-build.png)

 Finally, we glance the Glance.
 
  ![image](http://nilstack.github.io/public/image/glance_weather.png)

You can get the complete project from [Github](https://github.com/NilStack/HierarchicalWatch).

---

## From the author

--

This is the fourth part of iWatch App Development Basic. Please help me share it.

<a href="https://twitter.com/share" class="twitter-share-button" data-via="NilStack" data-size="large" data-hashtags="WatchKit">Tweet</a>

<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>




