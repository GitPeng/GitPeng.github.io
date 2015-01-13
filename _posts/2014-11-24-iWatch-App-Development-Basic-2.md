---
layout: post
title: iWatch App Development Basic 2
---

There're two styles for presenting multiple scenes in WatchKit: hierarchical and page-based. We start with the second one because it's simpler.

The page-based scenes are like Weather app in iOS to present the informations one by one.Please repeat step 1 and 2 in the previous article to create new project and add watch target.

### add more interface controllers

---

 Please add more interface controllers for your requirement by dragging and dropping Interface Controller in object library.
 
 ![image](http://nilstack.github.io/public/image/add-more-interface-controllers.png)

### create corresponding interface controllers

---

Please create subclasses of WKInterfaceController in WatchKit Extension to config the Interface Controller you set up in previous step.

 ![image](http://nilstack.github.io/public/image/add_interface_controllers.png)
 
 Then change the class of Interface Controller in storyboard to their corresponding subclass.
 
 ![image](http://nilstack.github.io/public/image/config_interface_controller.png)

Finally put label in every controller to tell apart and connect the IBOutlet in every class.

### create segue to connect every interface controller 

---

Please drag and drop one interface controller to another and select next page in popup.

![image](http://nilstack.github.io/public/image/connect_next_page.png)

### build and run

---

<iframe width="420" height="315" src="//www.youtube.com/embed/tH85tUTLCSE" frameborder="0" allowfullscreen></iframe>
 
You can get the complete project from [Github](https://github.com/NilStack/pagewatch).

# Note: All the contents and new coming posts are moving to [http://develop.watch](http://develop.watch). Thanks for your support.

---

## From the author

--

This is second part of iWatch App Development Basic. Please share it.

<a href="https://twitter.com/share" class="twitter-share-button" data-via="NilStack" data-size="large" data-hashtags="WatchKit">Tweet</a>

<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>

## Subscribe

---

<!-- Begin MailChimp Signup Form -->
<link href="//cdn-images.mailchimp.com/embedcode/classic-081711.css" rel="stylesheet" type="text/css">
<style type="text/css">
	#mc_embed_signup{background:#fff; clear:left; font:14px Helvetica,Arial,sans-serif; }
	/* Add your own MailChimp form style overrides in your site stylesheet or in this style block.
	   We recommend moving this block and the preceding CSS link to the HEAD of your HTML file. */
</style>
<div id="mc_embed_signup">
<form action="//github.us9.list-manage.com/subscribe/post?u=ff5dae3ddc1f4cead9b9d7277&amp;id=868c3a1b23" method="post" id="mc-embedded-subscribe-form" name="mc-embedded-subscribe-form" class="validate" target="_blank" novalidate>
    <div id="mc_embed_signup_scroll">
	<h2>Welcome subscribe to Watch App Dev Weekly mailing list</h2>
<div class="mc-field-group">
	<label for="mce-EMAIL">Email Address </label>
	<input type="email" value="" name="EMAIL" class="required email" id="mce-EMAIL">
</div>
	<div id="mce-responses" class="clear">
		<div class="response" id="mce-error-response" style="display:none"></div>
		<div class="response" id="mce-success-response" style="display:none"></div>
	</div>    
    <div style="position: absolute; left: -5000px;"><input type="text" name="b_ff5dae3ddc1f4cead9b9d7277_868c3a1b23" tabindex="-1" value=""></div>
    <div class="clear"><input type="submit" value="Subscribe" name="subscribe" id="mc-embedded-subscribe" class="button"></div>
    </div>
</form>
</div>
<script type='text/javascript' src='//s3.amazonaws.com/downloads.mailchimp.com/js/mc-validate.js'></script><script type='text/javascript'>(function($) {window.fnames = new Array(); window.ftypes = new Array();fnames[0]='EMAIL';ftypes[0]='email';fnames[1]='FNAME';ftypes[1]='text';fnames[2]='LNAME';ftypes[2]='text';}(jQuery));var $mcj = jQuery.noConflict(true);</script>

---






 
