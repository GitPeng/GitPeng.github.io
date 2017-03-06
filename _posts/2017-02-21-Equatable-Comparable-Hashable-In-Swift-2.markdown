---
layout: post
title:  "Swift World: Equatable, Comparable and Hashable Part 2"
date:   2017-02-21 20:00:00
categories: tutorial
---

In the last article, we introduced protocol Equatable. Today protocol Comparable will be covered. The Integer type is inherently comparable like 5 < 7. How about our self-defined types? How to bring order to superheroes we defined in [Part 1](http://pengguo.xyz/tutorial/2017/02/19/Equatable-Comparable-Hashable-In-Swift-1.html) ?  Is superman > batman possible in Swift world?

We will follow our procedure to show Comparable’s source code first.
The following codes are from [Comparable.swift](https://github.com/apple/swift/blob/master/stdlib/public/core/Comparable.swift)

```swift
public protocol Comparable : Equatable {
    static func < (lhs: Self, rhs: Self) -> Bool
    static func <= (lhs: Self, rhs: Self) -> Bool
    static func >= (lhs: Self, rhs: Self) -> Bool
    static func > (lhs: Self, rhs: Self) -> Bool
}
```

Four relational operators are defined in this protocol, but we only need to implement < in conformance to Comparable. The other three operators has been given default implementations.

```swift
public func > <T : Comparable>(lhs: T, rhs: T) -> Bool {
  return rhs < lhs
}
```

```swift
public func <= <T : Comparable>(lhs: T, rhs: T) -> Bool {
  return !(rhs < lhs)
}
```

```swift
public func >= <T : Comparable>(lhs: T, rhs: T) -> Bool {
  return !(lhs < rhs)
}
```

Let’s enhance our Superhero type to give each hero a power value.

```swift
struct Superhero {
    let firstName: String
    let lastName: String
    let age: Int
    var power: Int
}
```

And update Superman and Batman.

```swift
let superman = Superhero(firstName: "Clark", lastName: "Kent", age: 35, power: 100)
let batman = Superhero(firstName: "Bruce", lastName: "Wayne", age: 32, power: 90)
```

Ok, maybe you don’t agree with me and Batman is top one on your list. Never mind change the value as you wish.

In the world built by me in Swift, superheroes are ordered by their power value.

```swift
extension Superhero: Comparable {

    static func < (lhs: Superhero, rhs: Superhero) -> Bool {
        return lhs.power < rhs.power
    }

    static func == (lhs: Superhero, rhs: Superhero) -> Bool {
        return lhs.firstName == rhs.firstName &&
            lhs.lastName == rhs.lastName &&
            lhs.age == rhs.age
    }
}
```

As we mentioned, Comparable extends Equatable, so we need to implement == at the same time.

Then let’s  start Before & After comparison.

Before:

```swift
let superheroes = [superman, batman]
```

```swift
let isSupermanTopOne = superman.power > batman.power

superheroes.sorted(by: {$0.power < $1.power })
```

After:

```swift
let isSupermanTopOne = superman.power > batman.power

superheroes.sorted()
```

In the next article, we will introduce protocol Hashable.
