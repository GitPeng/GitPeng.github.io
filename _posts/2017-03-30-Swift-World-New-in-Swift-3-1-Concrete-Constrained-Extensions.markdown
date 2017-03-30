---
layout: post
title:  "Swift World: New in Swift 3.1 - Concrete Constrained Extensions"
date:   2017-03-30 20:00:00
categories: tutorial
---

Today, we’ll talk about concrete constrained extensions and Availability by Swift Version. Simply speaking, it helps extend concrete type with concrete constraint.  In the [change log](https://github.com/apple/swift/blob/master/CHANGELOG.md) of Swift, a simple definition is given as following.

```
extension Array where Element == Int { }
```

It extends Array with concrete type Int which means the extension will only works on Int. Let’s show a simple extension to get the sum of elements in the array.

```swift
extension Array where Element == Int {
    func sum() -> Int {
        return reduce(0, +)
    }
}

[1, 2, 4, 8].sum() // 15
```

In Swift 3.0, we can only use a protocol as constraint to extend concrete type. We will not give an example but you can find many example on Internet.

Thanks for your time.
