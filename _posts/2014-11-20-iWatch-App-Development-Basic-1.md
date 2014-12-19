---
layout: post
title: iWatch App Development Basic 1
---

Watchkit was released finally.

This is the first part of a serial tutorial.
Please download Xcode 6.2 Beta with an Apple developer account.

Let's start the new journey.

Currently, you can't develop native app for iWatch.

![image](http://nilstack.github.io/public/image/target_structure_2x.png)

As the diagram shows, there are three parts for a runnable watch app: containing iOS app, watchkit extension and watch app. They are all bundled in an iOS app.

### create new project

Let's start with a simple single view app with Xcode project because there is no content in the containing app.

![image](http://nilstack.github.io/public/image/create_new_project.png)


### add the watch target

Then please add a watch target to the containing app. Xcode will generate related code and resource automatically.

![image](http://nilstack.github.io/public/image/add_watch_target.png)

If you want to include a glance scene or notification scene, please select in the next step.

![image](http://nilstack.github.io/public/image/check_glance_and_notification.png)

The auto generated codes include WatchKit App and Watch  Extension. There are only UI related resources like storyboard in the watch app. Your codes are all in the watchkit extension which will run in your iOS device.

![image](http://nilstack.github.io/public/image/auto_generated_codes.png)


### add UI components

Please open Interface.storyboard in Watch App, then drag and drop a label and a button to the Interface Controller Scene.

![image](http://nilstack.github.io/public/image/add_ui_components.png)

### add code logic

Please open InterfaceController.m, then add these codes to it.

    #import "InterfaceController.h"


	@interface InterfaceController()
	@property (weak, nonatomic) IBOutlet WKInterfaceLabel *label;
	@property (weak, nonatomic) IBOutlet WKInterfaceButton *btn;
	
	@end
	
	
	@implementation InterfaceController
	
	- (instancetype)initWithContext:(id)context {
	    self = [super initWithContext:context];
	    if (self){
	        // Initialize variables here.
	        // Configure interface objects here.
	        NSLog(@"%@ initWithContext", self);
	        
	    }
	    return self;
	}
	- (IBAction)changeLabelTextColor {
	    [self.label setTextColor:[UIColor redColor]];
	
	}
	
	- (void)willActivate {
	    // This method is called when watch view controller is about to be visible to user
	    NSLog(@"%@ will activate", self);
	}
	
	- (void)didDeactivate {
	    // This method is called when watch view controller is no longer visible
	    NSLog(@"%@ did deactivate", self);
	}
	
	@end


Link the IBOutlet and IBAction to the storyboard. If you are not clear with it, please refer to the storyboard document.

### build and run

Select the scheme of watch app and run it.

![image](http://nilstack.github.io/public/image/select_scheme.png)

select Hardware->External Displays -> Apple Watch XXmm to  show the watch simlator.

![image](http://nilstack.github.io/public/image/watch_sim_before.png)

click the button, the text color will be changed to red.

![image](http://nilstack.github.io/public/image/watch_sim_after.png)

Hello iWatch!

You can get the complete project from [Github](https://github.com/NilStack/FirstWatch).

### Note: All the contents and new coming posts are moving to [http://develop.watch](http://develop.watch). Thanks for your support.



 
