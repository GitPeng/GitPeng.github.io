---
layout: post
title: iWatch App Development Basic 3
---

In the previous blog, I introduced page-based navigation in WatchKit. Now let's see the hierarchy one. As the experience from iOS app, table view is the best example to present hierarchical content. So i will also introduce table in WatchKit. 

Let's get started.

First, please create new containing iOS project and add watch app target as described in the previous articles.

### add table in the main Interface Controller

Please pull a table object to the main Interface Controller in the storyboard.

 ![image](http://nilstack.github.io/public/image/add_table.png)

Each table row has its corresponding table row controller in which a group resides. You can lay out other UI components in the group.This is the difference with the UITableView pattern in UIKit.  

Here, we will put a label and an image in the row.

First create new subclass of NSObject in WatchKit Extension because table row controller is a NSObject actually. Then change table row controller's class to your own in Identity Inspector. Fianlly add and link two IBOutlets in header file.
The result is here.

![image](http://nilstack.github.io/public/image/add_table_row_controller.png)

### add logic

Let's start to add the logic. Pleae add and link the table to the main interface controller. As the default logic in the UITableViewController, now you need a data source and config the rows. The codes are here.

    @interface InterfaceController()

    @property (strong, nonatomic) NSArray *appTypes;

    @end


    @implementation InterfaceController

    - (instancetype)initWithContext:(id)context {
    self = [super initWithContext:context];
    if (self){
        // Initialize variables here.
        // Configure interface objects here.
        NSLog(@"%@ initWithContext", self);
        
        self.appTypes = @[@"apps", @"glances", @"notifications"];
        
        [self.table setNumberOfRows:self.appTypes.count withRowType:@"MyTableRowController"];
        
        [self.appTypes enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
            
            MyTableRowController* row = [self.table rowControllerAtIndex:idx];
            
            [row.appType setText: (NSString*)obj];
            [row.logo setImage:[UIImage imageNamed:(NSString *)obj]];
            
        }];

        
    }
    return self;
    }
    ...
    
    @end
    
### create the detail Interface Controller

please pull a new Interface Controller to the storyboard besides the main one, create detail Interface Controller class and add an image object in it.


![image](http://nilstack.github.io/public/image/add_detail_interface_controller.png)

### create segue and transfer data

Please create a segue from table row controller to the detail Interfaece Controller and select push.

![image](http://nilstack.github.io/public/image/create_segue.png)

Add 

     - (instancetype) contextForSegueWithIdentifier:(NSString *)segueIdentifier inTable:(WKInterfaceTable *)table rowIndex:(NSInteger)rowIndex{
    
    return [self.appTypes objectAtIndex:rowIndex];
    }
    
to InterfaceController's implementation to send data to the detail controller.

In the DetailInterfaceController's init, add

     
        NSString *imageName = (NSString*)context;
        
        [self.image setImage: [UIImage imageNamed:imageName ]];
        
to set the image source.

### build and run

![image](http://nilstack.github.io/public/image/table_view.png)

![image](http://nilstack.github.io/public/image/detail_image.png)


You can get the complete project from [Github](https://github.com/NilStack/HierarchicalWatch).

---

## From the author

--

This is third part of iWatch App Development Basic. Please share it.

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

 


