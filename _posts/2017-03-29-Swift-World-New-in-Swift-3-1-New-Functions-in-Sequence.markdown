---
layout: post
title:  "Swift World: New in Swift 3.1 - New Functions in Sequence"
date:   2017-03-29 20:00:00
categories: tutorial
---

Swift 3.1 was released with Xcode 8.3 and iOS 10.3. It brings many interesting features. In this article, we will introduce two new functions prefix(while:) and drop(while:)  in Sequence.

The proposal for these two functions is [SE-0045](https://github.com/apple/swift-evolution/blob/master/proposals/0045-scan-takewhile-dropwhile.md). It describes the motivation and design for prefix and drop. Please dive in if you want to learn cause and effect. We will only explain the usage with examples.

Let’s get started!

What the prefix(while:) does is to return the prefix elements which match the predicate and stop at the first element which doesn’t match. An example is worth a thousand words.

Let’s assume we have a group of person’s ages in a list.

```swift
let ages = [12, 20, 15, 8, 16, 45, 33, 28, 38, 50]
let prefixed = ages.prefix { $0 < 40 }
print(prefixed)
```

We can get the result  [12, 20, 15, 8, 16]. It doesn’t contain the elements which are great than 40 but come after 45.

What drop(while:) does is to drop the elements which match the predicate util the first non-matching and keep all after it.

```swift
let ages = [12, 20, 15, 8, 16, 45, 33, 28, 38, 50]
let dropped = ages.drop { $0 < 40 }
print(dropped)
```

We get the result [45, 33, 28, 38, 50] finally. The elements from12 to 16 are dropped. 45 and the elements come after it are all kept.

In next blog, we will introduce concrete constrained extensions. Thanks for your time.
