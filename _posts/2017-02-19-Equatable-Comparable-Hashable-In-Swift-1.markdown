---
layout: post
title:  "Swift World: Equatable, Comparable, and Hashable in Swift Part 1"
date:   2017-02-19 20:00:00
categories: tutorial
---

In this series, we will learn three basic protocols: Equatable, Comparable and Hashable. They will be explained with examples from Swift’s codebase and our own.

Let’s see how protocol Equatable is defined in Swift. The following snippet is from [stdlib  of Swift] ( [swift/Equatable.swift at master · apple/swift · GitHub](https://github.com/apple/swift/blob/master/stdlib/public/core/Equatable.swift) ). There is only a function for operator == with two parameters which mean left-hand side and right-hand side value.

```
public protocol Equatable {
  static func == (lhs: Self, rhs: Self) -> Bool
}
```

If we want to make a type equatable, we only need to implement the operator. The following is an implementation for UIEdgeInsets from [UIKit in Swift](https://github.com/apple/swift/blob/master/stdlib/public/SDK/UIKit/UIKit.swift) .  If two UIEdgeInsets’ top, left, bottom and right are equal, they’re considered equal.

```
public func == (lhs: UIEdgeInsets, rhs: UIEdgeInsets) -> Bool {
  return lhs.top == rhs.top &&
         lhs.left == rhs.left &&
         lhs.bottom == rhs.bottom &&
         lhs.right == rhs.right
}
```

Then what if we define our own type like the following? The person type has  three properties: first name, last name and age.

```
struct Superhero {
    let firstName: String
    let lastName: String
    let age: Int
}
```

According to our own rules, if a person’s first name, last name and age are equal to the other’s, we consider they are equal.

```
extension Superhero {
    static func == (lhs: Superhero, rhs: Superhero) -> Bool {
        return lhs.firstName == rhs.firstName &&
               lhs.lastName == rhs.lastName &&
               lhs.age == rhs.age
    }
}
```

What’s the advantage to conform to Equatable?  The quote from the comment for this protocol is very clear. “Some sequence and collection operations can be used more simply when the elements conform to `Equatable`.” Let’s explain it with examples.

Before conforming to Equatable,  we need predicate.

```
let superman = Superhero(firstName: "Clark", lastName: "Kent", age: 35)
let batman = Superhero(firstName: "Bruce", lastName: "Wayne", age: 32)

let superheroes = [superman, batman]
let isSupermanInSuperheroes = superheroes.contains {
    return $0.firstName == "Clark" &&
           $0.lastName == "Kent" &&
           $0.age == 35
}
```

After conforming to Equatable

```
let isSupermanInSuperheroes = superheroes.contains(superman)
```

In the next article, we will introduce protocol Comparable.
