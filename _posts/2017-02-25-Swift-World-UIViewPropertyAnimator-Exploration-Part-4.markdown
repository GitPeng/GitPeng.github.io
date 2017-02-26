---
layout: post
title:  "Swift World: UIViewPropertyAnimator Exploration Part 4"
date:   2017-02-26 20:00:00
categories: tutorial
---

In this part, we will introduce Dance which is built upon UIViewPropertyAnimator. We will not talk about what it is and how to use it because [its document](https://github.com/saoudrizwan/Dance) is very clear. On the contrary, how it is built is the main topic in this part.It means we will explain the core logic inside Dance, so you can take it as an example of UIViewPropertyAnimator.

In Dance’s document, there is a main example to explain its usage. We quote it here.

```
let circle = UIView()
...
circle.dance.animate(duration: 2.0, curve: .easeInOut) {
    $0.transform = CGAffineTransform(scaleX: 1.5, y: 1.5)
    $0.center = self.view.center
    $0.backgroundColor = .blue // ... see 'Animatable Properties' for more options
}.addCompletion { _ in
    self.view.backgroundColor = .green
}.start(after: 5.0)
```

The following codes are the same animation with UIViewPropertyAnimator.

```
let animator = UIViewPropertyAnimator(duration: 2.0, curve: .easeInOut) {
    circle.transform = CGAffineTransform(scaleX: 1.5, y: 1.5)
    circle.center = self.view.center
    circle.backgroundColor = .blue
}
animator.addCompletion { _ in
    self.view.backgroundColor = .green
}
animator.startAnimation(afterDelay: 5.0)
```

UIViewPropertyAnimator builds animation from animation’s view. So we need to build animator first,  then add view and its change to the animator in a block. Dance builds animation from UIView’s view and organizes the animation code in another way.

Let’s get started to see how Dance accomplish this.

### UIView extension
In this extension, Dance defines a computed property with associated object. This property gives every UIView a Dance instance. That‘s where story starts. You will get what happens with `circle.dance`.  

```
@available(iOS 10.0, *)
extension UIView {

    public var dance: Dance {
        get {
            return DanceExtensionStoredPropertyHandler.associatedObject(base: self, key: &DanceExtensionStoredPropertyHandler.danceKey) {
                return Dance(dancingView: self) // initial value - is reference type so it won't change
            }
        }
        set {
            DanceExtensionStoredPropertyHandler.associateObject(base: self, key: &DanceExtensionStoredPropertyHandler.danceKey, value: newValue)
        }
    }

}
```

### DanceFactory
Before going to `circle.dance.animate()`, let’s see DanceFactory first. DanceFactory works as an UIViewPropertyAnimator producer and wrapper.  There’re several createNewAnimator functions with different parameters like UIViewPropertyAnimator’s different initializers. The difference is DanceFactory gives every UIViewPropertyAnimator a tag as key and stores it in a dictionary. Take one of them as example.

```
func createNewAnimator(tag: Int, duration: TimeInterval, timingParameters: UITimingCurveProvider, animations: @escaping (() -> Void)) {
    let newAnimator = UIViewPropertyAnimator(duration: duration, timingParameters: timingParameters)
    newAnimator.addAnimations(animations)
    newAnimator.addCompletion { (_) in
        self.animators.removeValue(forKey: tag)
    }
    animators[tag] = newAnimator
}
```

 As a wrapper, the factory wraps UIViewPropertyAnimator’s normal functions like startAnimation, pauseAnimation, finishAnimation, etc. It delegate the work to the animator with specific tag it creates in createNewAnimator.

```
func startAnimation(tag: Int) {
    if let animator = animators[tag] {
        animator.startAnimation()
    } else {
        handle(error: .noAnimation, forViewWithTag: tag)
    }
}
```

### Dance

Although Dance is the core of the framework, it actually delegates most its work to DanceFactory like:

```
 @discardableResult public func animate(duration: TimeInterval, curve: UIViewAnimationCurve, _ animation: @escaping (make) -> Void) -> Dance {
        …
    DanceFactory.instance.createNewAnimator(tag: self.tag, duration: duration, curve: curve) {
        animation(self.dancingView)
    }
    return dancingView.dance
 }
```

and

```
@discardableResult public func start() -> Dance {
        DanceFactory.instance.startAnimation(tag: self.tag)
        return dancingView.dance
}
```

Then we can figure out what happens with `circle.dance.animate()`. The circle view ‘has a property’ dance which is an instance of Dance. Simply speaking, when it calls animate, DanceFactory’s singleton instance will create a new animator with the animation and call this animator’s startAnimation in Dance’s start(). DanceFactory manages all UIViewPropertyAnimator instances.

Dance has other advantages like function chaining and debugging. You can get the details from its complete [document](https://github.com/saoudrizwan/Dance). It’s an good example to improve the experience with simple strategy.

Thanks for your time.
