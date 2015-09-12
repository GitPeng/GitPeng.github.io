---
layout: post
title:  "Interaction With Apple TV I : What is focus"
date:   2015-09-12 18:00:00
categories: tutorial
---
On Apple TV, the interaction is very different because there is no touch screen. The Apple Remote is the main physical device users can operate and interact with UI.

>  ![Apple Remote](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/Art/remote_callouts_2x.png)
>  From App Programming Guide for tvOS

There are (1)touchpad and many functional buttons including (2)Menu, (3)Siri/Search, (4)Play/Pause, (5)Home and (6)Volume. We can get what they do with their names.

There are two stages if you want to interact with the UI component on screen: focus and action. For example, we need to navigate to a button first when we want to tap it. The effect when the navigation completes is called focus in tvOS. It's not so easy like the mouse to desktop and gesture to touch screen. We know the explanation is a little confusing but you'll know it when you see it.

 ![focus](https://db.tt/Qwi8FRAN)

 In the video, the initial focus is on the Settings.app. With the shadow, the icon seems to float over the screen. Then with the Option key is pressed down on keyboard the mouse is moved to left of the touchpad, we can see the focus is also moved to the Setting's left neighbours. The moving stands for swiping or panning on a real Apple Remote. When we travel back to focus Settings again, Settings is opened by clicking which stands for tapping.

 Ok, now we know what focus looks like on Apple TV. The focus model is key in interaction of tvOS. It is similar to the response chain in iOS. We will introduce this model in next article.

 Happy weekends!
