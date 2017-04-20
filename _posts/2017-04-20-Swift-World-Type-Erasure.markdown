---
layout: post
title:  "Swift World: Type Erasure"
date:   2017-04-20 20:00:00
categories: tutorial
---

> Swift is a Protocol-Oriented Programming Language.
>                             - from WWDC 2015 Protocol-Oriented Programming in Swift

> Generics are one of the most powerful features of Swift, and much of the Swift standard library is built with generic code.
>                             - from The Swift Programming Language

I start this article with two quotes on protocol and generics. Both are core features of Swift.  As we know, there are generics for class, struct and enum. What about protocol?

Suppose we’re working on a cache framework for our app. For different data type being cached, we often make them conform to an interface as  following Cacable protocol.

```swift
protocol Cachable<T> {}
```

We will get “error: protocols do not allow generic parameters; use associated types instead”. This error message leads us to associated types.

> An associated type gives a placeholder name to a type that is used as part of the protocol.  
>                                      - from The Swift Programming Language

Ok, let’s follow the suggestion to use associated type. The following codes are from an open source cache framework - [GitHub - hyperoslo/Cache](https://github.com/hyperoslo/Cache).

```swift
protocol Cachable {
    associatedtype CacheType

    func decode(_ data: Data) -> CacheType?
    func encode() -> Data?
}
```

But we all know Swift is a strongly typed.  So  

> The actual type to use for that associated type isn’t specified until the protocol is adopted.
>                                        - from The Swift Programming Language

When a type conforms to the protocol with associatedtype, it needs to make associatedtype clear. As shown in the following snippet, the associatedtype is defined as String by  `typealias CacheType = String ` .

```swift
extension String: Cachable {

    typealias CacheType = String

    func decode(_ data: Data) -> CacheType? {
        guard let string = String(data: data, encoding: String.Encoding.utf8) else {
            return nil
        }

        return string
    }

    func encode() -> Data? {
        return data(using: String.Encoding.utf8)
    }
}
```

This is protocol’s own mechanism for generics - Protocol with Associated Types (PATs). [@NatashaTheRobot](https://twitter.com/NatashaTheRobot)’s article [Swift: What are Protocols with Associated Types?](https://www.natashatherobot.com/swift-what-are-protocols-with-associated-types/#)  is a simple and clear explanation.  Dive in [Alexis Gallagher’s talk](https://www.youtube.com/watch?v=XWoNjiSPqI8) if you want to get deep and complete understanding on PATs.

The world seems perfect because finally we have generics for protocol. But in following cases, PATs have troubles. Suppose we have CacheItem which wraps a Cachable item and its expiry date.

```swift
struct CacheItem {
    let item: Cachable
    let expiryDate: Date
    ...
}
```

We will get error as below.

` error: protocol 'Cachable' can only be used as a generic constraint because it has Self or associated type requirements`

Or we will get same error when we want to define an array for Cachable.

```swift
let cache: [Cachable]
```

Swift’s code base is a big treasure where we can find solution for these cases. The technique is type erasure. One of the examples is AnySequence which is type-erased wrapper for Sequence. Its internal mechanism is described as following quote.

> An instance of AnySequence forwards its operations to an underlying base sequence having the same Element type, hiding the specifics of the underlying sequence.
>                                    - from Swift Standard Library API Reference

So we need AnyCachable for Cachable.

```swift
class AnyCachable<T>: Cachable {    
    private let _decode: (_ data: Data) -> T?
    private let _encode: () -> Data?

    init<U: Cachable>(_ cachable: U) where U.CacheType == T {
        _decode = cachable.decode
        _encode = cachable.encode
    }

    func decode(_ data: Data) -> T? {
        return _decode(data)
    }

    func encode() -> Data? {
        return _encode()
    }
}
```

The main points are:

1. AnyCachable is a generic type conforming to Cachable.  
2. The type parameter T is also Cachable’s associated type.
3. Define internal closures for functions and properties of the protocol Cachable.
4. Forward the operations to the wrapped Cachable instance.

Then we can solve the problems as shown below.

```swift
struct CacheItem {
    let item: AnyCachable<Any>
    let expiryDate: Date
    ...
}
```

```swift
let cache: [AnyCachable<String>] = [AnyCachable("Hello"), AnyCachable("World")]
```

There are other implementations for type erasure like Robert Edwards described in [Breaking Down Type Erasure in Swift](https://www.bignerdranch.com/blog/breaking-down-type-erasures-in-swift/).

Finally, I will recommend other resources on type erasure.

* [Keep Calm and Type Erase On](https://realm.io/news/tryswift-gwendolyn-weston-type-erasure/) by [Gwendolyn Weston](https://twitter.com/purpleyay)
* [Swift: Attempting to Understand Type Erasure](https://www.natashatherobot.com/swift-type-erasure/) by [NatashaTheRobot](https://twitter.com/NatashaTheRobot)
* [Type erasure using closures in Swift](https://medium.com/@johnsundell/type-erasure-using-closures-in-swift-1e6125231f8) by [John Sundell](https://twitter.com/johnsundell)

Thanks for your time.
