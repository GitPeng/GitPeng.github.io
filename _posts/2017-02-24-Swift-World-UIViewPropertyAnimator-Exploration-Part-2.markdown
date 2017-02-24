---
layout: post
title:  "Swift World: UIViewPropertyAnimator Exploration Part 2"
date:   2017-02-24 20:00:00
categories: tutorial
---

In part 1, we began to talk about the dynamic characteristic of UIViewPropertyAnimator and gave an example to show how to interrupt animation. We will continue this topic and show more dynamic examples.

### Reverse animation

It’s not easy to reverse a running animation with old tools like block-based UIView animation although there is a limited autoreverse option in UIViewAnimationOptions. This option only can be used with the repeat option and only be triggered automatically. UIViewPropertyAnimator gives you the ability to reverse an animation whenever you want by only setting isReverse to true.

```
propertyAnimator.isReversed = true
```

Then the running animation is reversed.

! [ReverseAnimation.gif](https://github.com/NilStack/nilstack.github.io/blob/master/resources/ReverseAnimation.gif)

### Scrub animation

Based on the property fractionComplete which means what percentage of the animation has completed, we can change the progress of the animation.  Please pause the animation before change the value. In real world app, we can change the value according to a gesture’s position to make the animation more interactive. To make it easy to demonstrate, we change the value with the changing of slider’s value in our example.

```
func sliderValueChanged(_ sender: UISlider) {
        if propertyAnimator.isRunning {
            propertyAnimator.pauseAnimation()
        }
        propertyAnimator.fractionComplete = CGFloat(sender.value)
 }
```

 The progress of the animation is changed as following.

![ScrubAnimation.gif](https://github.com/NilStack/nilstack.github.io/blob/master/resources/ScrubAnimation.gif)

The complete codebase can be found [here](https://gist.github.com/NilStack/dee4247b04541762fecc0b93bdc0d251) .  In the next part, we will introduce timing curves which control the animation’s procedure.
