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

# Note: All the contents and new coming posts are moving to [http://develop.watch](http://develop.watch). Thanks for your support.

---

## From the author

--

This is third part of iWatch App Development Basic. Please share it.

<a href="https://twitter.com/share" class="twitter-share-button" data-via="NilStack" data-size="large" data-hashtags="WatchKit">Tweet</a>

<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>
