---
layout: post
title:  " Swift World: New in Swift 3.1 - Availability by Swift Version"
date:   2017-04-06 20:00:00
categories: tutorial
---

Swift is moving on. To support different incompatible versions, we use \#if swift(>= N) before Swift 3.1.  For example the following simple code snippet, we print different information in different versions.

```swift
#if swift(>=3.1)
    print("Hello Swift 3.1!")
#elseif swift(>=3.0)
    print("Hello Swift 3.0!")
#endif
```

In Swift 3.1, we use available attributes to make it more flexible. If you don’t familiar with attribute, the [official document](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Attributes.html) is a good reference. This article [Availability Attributes in Swift](https://www.raywenderlich.com/139077/availability-attributes-swift)  from raywenderlich.com explains the availability attributes clearly.

Let’s get started with a simple example.

```swift
@available(swift 3.1)
func isSwift31() -> Bool{
    return true
}
```

Then make it a little more complex.

```swift
@available(swift, introduced: 3.0, obsoleted: 3.1)
func isSwift30() -> Bool{
    return true
}
```

Obviously, it means `isSwift30()` was introduced in Swift 3.0 and is obsoleted in Swift 3.1. Call `isSwift30()`  in Swift 3.1 context will lead to error `note: 'isSwift30()' was obsoleted in Swift 3.1`.

Finally, the motivation and design details are in [SE-0141](https://github.com/apple/swift-evolution/blob/master/proposals/0141-available-by-swift-version.md) .  Dive in if you want to learn more.

Thanks for your time.
