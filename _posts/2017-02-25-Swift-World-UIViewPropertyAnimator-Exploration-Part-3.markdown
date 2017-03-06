---
layout: post
title:  "Swift World: UIViewPropertyAnimator Exploration Part 3"
date:   2017-02-25 20:00:00
categories: tutorial
---
As we mentioned in part 2, we will talk about  timing curves in this part.
Let’s back to the old days first. In block-based animation for UIView, there is option to set time curve like EaseInOut, EaseIn, EaseOut and  Linear. Time curve, time function or easing function means a function which defines the rate of changing in animation. For example
Talk is cheap, show me the animation.

![EaseInOut1.gif](http://pengguo.xyz/resources/EaseInOut1.gif)

And show me the code.

```swift
UIView.animate(withDuration: 4.0, delay: 0, options: [.curveEaseInOut], animations: {
            redRect.center.x = 300
        }, completion: nil)
```

The option .curveEaseInOut shows the timing curve option in the animation.

For CAAnimation, the corresponding property is timingFunction which is a CAMediaTimingFunction. To get same easeInOut effect, the code would be as the following.

```swift
let animation = CABasicAnimation()
animation.keyPath = "position.x"
animation.fromValue = redRect.center.x
animation.toValue = 300
animation.duration = 4
animation.timingFunction = CAMediaTimingFunction(name:
kCAMediaTimingFunctionEaseInEaseOut)
redRect.layer.add(animation, forKey: "EaseInOut")
```

When it comes to UIViewPropertyAnimator, we can also set a curve simply when creating an instance.

```swift
let animator = UIViewPropertyAnimator(duration: 4.0, curve: .easeInOut) {
    redRect.center.x = 300
}
```

You can change the value to easeInOut, easeIn, easeOut or linear to get different effects.

UIViewProperty also gives other methods to define timing curve.

1. Provide two control point to define cubic Bézier timing curve.

```swift
let animator = UIViewPropertyAnimator(duration: 4.0, controlPoint1: CGPoint(x: 0.17, y: 0.52), controlPoint2: CGPoint(x: 0.83, y: 0.67)) {
    redRect.center.x = 300
}
```

![ControlPoints](http://pengguo.xyz/resources/ControlPoint.gif)

2. Provide dampingRatio value to define spring-based timing curve.

```swift
let animator = UIViewPropertyAnimator(duration: 4.0, dampingRatio: 0.5) {
    redRect.center.x = 300
}
```

![CubicTiming.gif](http://pengguo.xyz/resources/CubicTiming.gif)

3. Provide an UITimingCurveProvider to define the cubic curve

```swift
let cubicTimingParameters: UITimingCurveProvider = UICubicTimingParameters(controlPoint1: CGPoint(x: 0.0, y: 1.0), controlPoint2: CGPoint(x: 1.0, y: 0.0))

let animator = UIViewPropertyAnimator(duration: 4.0, timingParameters: cubicTimingParameters)

animator.addAnimations {
    redRect.center.x = 300
}
animator.startAnimation()
```


![CubicTiming.gif](http://pengguo.xyz/resources/CubicTiming.gif)

4.  Provide an UITimingCurveProvider to define the spring timing

```swift
let springTimingParameters: UITimingCurveProvider = UISpringTimingParameters(dampingRatio: 0.5,initialVelocity: CGVector(dx:1.0, dy: 0.0))

let animator = UIViewPropertyAnimator(duration: 4.0, timingParameters: springTimingParameters)

animator.addAnimations {
    redRect.center.x = 300
}
animator.startAnimation()
```

![SpringTiming.gif](http://pengguo.xyz/resources/SpringTiming.gif)

In next part, we will introduce a third-party framework [Dance}(https://github.com/saoudrizwan/Dance) which is “ a powerful and straightforward animation framework built upon the new UIViewPropertyAnimator class ”.
