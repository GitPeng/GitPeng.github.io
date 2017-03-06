---
layout: post
title:  "Swift World: UIViewPropertyAnimator Exploration Part 1"
date:   2017-02-23 20:00:00
categories: tutorial
---

Apple introduced UIViewPropertyAnimator in last year’s WWDC.  It brings many advantages to make building interactive and interruptible animations more easily. We will look back at the old days first before exploring this  powerful tool.

Simply, we categorize the animation APIs into two levels. The higher one is for UIView directly.

```swift
+ (void)animateWithDuration:(NSTimeInterval)duration
                 animations:(void (^)(void))animations
                 completion:(void (^)(BOOL finished))completion;
```

Here is an example to change background from white to red. You can copy the codes and paste in Playground.

```swift
import PlaygroundSupport

let view = UIView(frame: CGRect(x: 0, y: 0, width: 200, height: 200))
view.backgroundColor = UIColor.white

UIView.animate(withDuration: 2.0, delay: 0, options: [.repeat, .autoreverse, .curveEaseInOut], animations: {
    view.backgroundColor = UIColor.red
}, completion: nil)

PlaygroundPage.current.liveView = view
```

In iOS 7, Apple released UIDynamicAnimator  which brought physics to UIView animation. [The official document](https://developer.apple.com/reference/uikit/uidynamicanimator) is a good reference.

The API in lower level is Core Animation for CALayer. We can define basic animation, keyframe animation, animation group and transition. The following is code snippet to accomplish the same effect as the previous example.

```swift
import PlaygroundSupport

let view = UIView(frame: CGRect(x: 0, y: 0, width: 200, height: 200))
view.backgroundColor = UIColor.red

let animation = CABasicAnimation(keyPath: "backgroundColor")
animation.duration = 2.0
animation.fromValue = UIColor.white.cgColor
animation.toValue = UIColor.red.cgColor
animation.repeatCount = 10
animation.autoreverses = true
animation.timingFunction = CAMediaTimingFunction(name: kCAMediaTimingFunctionEaseInEaseOut)

view.layer.add(animation, forKey: "BackgroundColorAnimation")

PlaygroundPage.current.liveView = view
```

The final effect is like:

 ![Swift-World-UIViewPropertyAnimator-animation1](http://pengguo.xyz/resources/Swift-World-UIViewPropertyAnimator-animation1.gif)

Time goes by and UIViewPropertyAnimator came with iOS 10. We quote the  definition from [the official reference](https://developer.apple.com/reference/uikit/uiviewpropertyanimator) which says UIViewPropertyAnimator “Animates changes to views and allows the dynamic modification of those animations.”
The first part is easy to understand because animation block for UIView does the same thing to “animate changes to views”. The following code snippet is to accomplish the same effect as the previous examples. The difference is we need to call startAnimation() unlike the previous solutions which start the animation automatically.

```swift
UIViewPropertyAnimator(duration: 2.0, curve: .easeInOut) {
        view.backgroundColor = UIColor.red
}.startAnimation()
```

What does it mean with “dynamic modification”?  The first point is we get the control to decide when to start, pause, resume and stop the animation. Let’s start a more complete example to explain it. The final effect is shown here.

![Swift-World-UIViewPropertyAnimator-animation2](http://pengguo.xyz/resources/Swift-World-UIViewPropertyAnimator-animation2.gif)

The complete codebase can be found [here](https://gist.github.com/NilStack/dee4247b04541762fecc0b93bdc0d251) . Let’s explain the main parts. We built animation to move a red rectangle.

```swift
let rect = UIView()
rect.frame = CGRect(x: 0, y: 50, width: 100, height: 100)
rect.backgroundColor = UIColor.red

...

let propertyAnimator: UIViewPropertyAnimator = UIViewPropertyAnimator(duration: 5.0, curve: UIViewAnimationCurve.easeInOut)

propertyAnimator.addAnimations {
    self.rect.center.x = 300
}
```

Start it by

```swift
propertyAnimator.startAnimation()
```

Pause it by

```swift
propertyAnimator.pauseAnimation()
```

Stop it by

```swift
propertyAnimator.stopAnimation(false)
```

It means the animation is interruptible and we can start, pause and stop it whenever we want to.

 In next article, we will talk about other dynamic advantages UIViewPropertyAnimator give us including adding animations flexibly,  reversing the animation, modify properties like timing curve while running, etc.
