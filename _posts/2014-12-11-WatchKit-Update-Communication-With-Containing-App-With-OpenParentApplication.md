---

layout: post
title: Communication With Containing App with openParentApplication:reply:

---

<!-- Begin MailChimp Signup Form -->
<link href="//cdn-images.mailchimp.com/embedcode/slim-081711.css" rel="stylesheet" type="text/css">
<style type="text/css">
	#mc_embed_signup{background:#BBDEFB; clear:left; font:18px Helvetica,Arial,sans-serif;  width:300px;}
	
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

Apple released iOS 8.2 beta 2 and Xcode 6.2 beta 2. The moste important update in WatchKit is these methods to build the bridge between the Watch app and its containing app. On Watch side:

    + (BOOL)openParentApplication:(NSDictionary *)userInfo
             reply:(void (^)(NSDictionary *replyInfo,
                             NSError *error))reply

and on containing app side:

    - (void)application:(UIApplication *)application               handleWatchKitExtensionRequest:(NSDictionary *)userInfo    reply:(void(^)(NSDictionary *replyInfo))reply
    
I did some research and built up a simple example. You can get the code from [Github](https://github.com/NilStack/OpenParentAppExample). In this example, the watch app will send "Hello" as a request to the containing app. Then the containing app will check the message and response with "WatchKit". 

Let's get started.
As usual, please set up a single view app and add Watch app target. In the watch app,  we a add label to show the request and response message and a button to trigger the sending action in the main Interface Controller.

![image](http://nilstack.github.io/public/image/openparentappmain.png)

Set the atrributes of the label and the button, then link them with IBOutlet and IBAction in the main Interface Controller class. 

    @interface InterfaceController()

    @property (weak, nonatomic) IBOutlet WKInterfaceLabel *label;
    @property (weak, nonatomic) IBOutlet WKInterfaceButton *btn;

    @end
    
    @implementation InterfaceController
    
    ...

    - (IBAction)sendRequest {
    
    NSDictionary *requst = @{@"request":@"Hello"};
    
    [InterfaceController openParentApplication:requst reply:^(NSDictionary *replyInfo, NSError *error) {
        
        if (error) {
            NSLog(@"%@", error);
        } else {
            
            [self.label setText:[replyInfo objectForKey:@"response"]];
        }
        
    }];
    
    ...
    }

Here you can see i define a NSDicionary as userInfo which stores the request message. The reply is a block which will execute when the response is received from the containing app. The replyInfo contains the reponse and the error means the communication failed if it's not nil. You can set the reply as nil if you don't want to handle the response.


In the app delegate of containing app, please add the code.

    - (void)application:(UIApplication *)application handleWatchKitExtensionRequest:(NSDictionary *)userInfo reply:(void (^)(NSDictionary *))reply{
    
         if ([[userInfo objectForKey:@"request"] isEqualToString:@"Hello"]) {
        
        NSLog(@"containing app received message from watch");
        
        NSDictionary *response = @{@"response" : @"Watchkit"};
        reply(response);
        }
    
    }
    
The app delegate will handle the userInfo i defined in the previous step and reply with the reponse in NSDictionary.

Build and run. When you tap the button, iOS will wake up your containing app in the background. But if you run the example with the simulator, the containing app will be pushed to the front. So i add a label to indicate the containing app is running.

Here is the recording for the example.

<iframe width="420" height="315" src="//www.youtube.com/embed/pkFT2BOJPtc" frameborder="0" allowfullscreen></iframe>

You can download the code from [Github](https://github.com/NilStack/OpenParentAppExample).

---

## From the author

---

Please help me share it.

<a href="https://twitter.com/share" class="twitter-share-button" data-via="NilStack" data-size="large" data-hashtags="WatchKit">Tweet</a>

<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>





