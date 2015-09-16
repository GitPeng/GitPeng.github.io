---
layout: post
title:  "Interaction With Apple TV I : What is focus"
date:   2015-09-12 18:00:00
categories: tutorial
---
This is a simple introduction of focus in tvOS. More details will come soon.

On Apple TV, the interaction is very different because there is no touch screen. The Apple Remote is the main physical device users can operate and interact with UI.

>  ![Apple Remote](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/Art/remote_callouts_2x.png)
>  From App Programming Guide for tvOS

There are (1)touchpad and many functional buttons including (2)Menu, (3)Siri/Search, (4)Play/Pause, (5)Home and (6)Volume. We can get what they do with their names.

There are two stages if you want to interact with the UI component on screen: focus and action. For example, we need to navigate to a button first when we want to tap it. The effect when the navigation completes is called focus in tvOS. It's not so easy like the mouse to desktop and gesture to touch screen. We know the explanation is a little confusing but you'll know it when you see it.

 ![focus](https://db.tt/Qwi8FRAN)

 In the video, the initial focus is on the Settings.app. With the shadow, the icon seems to float over the screen. Then with the Option key is pressed down on keyboard the mouse is moved to left of the touchpad, we can see the focus is also moved to the Setting's left neighbours. The moving stands for swiping or panning on a real Apple Remote. When we travel back to focus Settings again, Settings is opened by clicking which stands for tapping.

 Ok, now we know what focus looks like on Apple TV. The focus model is key in interaction of tvOS. It is similar to the response chain in iOS. We will introduce this model.

 Now we built another example. In the demonstration video, the swiping on the touchpad triggered the focus moving to left and the left neighbour was focused. This demonstrated the focus effect, movement and update.

  ![focus](https://db.tt/oWGECPL9)

 First, it is focus engine in system which is in charge of focus.

  The focus engine need an initial focus view as its start and then move the focus as user, app or system's requests. So it need to keep an UI hierarchy of the app. In the UI hierarchy, there is a hidden focus chain. The link is preferredFocusedView of UIFocusEnvironment protocol. If we don't override this method in UIView, UIViewController, UIWindow, and UIPresentationController which conform to UIFocusEnvironment, the focus engine will select the most top-left focusable view.

   ![default-initial-focus](https://db.tt/jPn8vwqN)

  If we change the initial focused view by adding

      - (UIView *)preferredFocusedView
      {
          return self.button3;
      }

  So the initial focused view goes to button3 when the app launches.

    ![preferredFocusedView](https://db.tt/OUQ3dALX)

   The whole process is

 >   ![Search-Focused-View](https://db.tt/XgAkcMR9)
 >   -From App Programming Guide for tvOS

 Then when to move focus to our target? The trigger is focus movement event from input devices is sent to tvOS. Apple Remote is the default input device. Others are like game controller and simulator. The focus movement event like swiping on touchpad to different directions will lead focus to corresponding directions.

 There are maybe several focusable views in the directions which focus heads. Which is the next one? The nearest focusable view on the path of motion. Then the shouldUpdateFocusInContext: of UIFocusEnvironment will be called. So override it if you want to do something.

 After the next target is selected, the system will update focus according to the user's operation it received, the update requests from app and some specific changes in UI.

 At this point, focus engine will call didUpdateFocusInContext:withAnimationCoordinator: to notify every focus environment and define the UIFocusAnimationCoordinator which used in movement.

 As we can see, the focus is moved with animation. In the first video, the current focus icon is likely dragged to the direction of movement. The light is also twinkling to the next focus view. We can customize the animation with UIFocusAnimationCoordinator.

 We will bring more details and examples soon.
