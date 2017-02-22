---
layout: post
title:  "Swift World: Equatable, Comparable, and Hashable Part 3"
date:   2017-02-22 20:00:00
categories: tutorial
---

In previous parts, we covered Equatable and Comparable. This article is the last one of this serial and we will talk about Hashable. The direct purpose to make our custom type hashable is to use it in a Set or Dictionary. But you can find more while you’re using Swift.

By convention, the source code of Hashable is shown at first.

```
public protocol Hashable : _Hashable, Equatable {
  var hashValue: Int { get }
}
```

It’s clear we only need to do is providing a hashValue to make our custom type hashable. So how to give the value? In Swift, Bool gives a very simple implement.

```
public var hashValue: Int {
    return self ? 1 : 0
  }
```

 A more common practice is to combine every property’s hash value with the bitwise XOR operator. Let’s see make our Superhero hashable in this way.

```
extension Superhero: Hashable {
    public var hashValue: Int {
        return firstName.hashValue ^ lastName.hashValue ^ age.hashValue ^ power.hashValue
    }

    static func == (lhs: Superhero, rhs: Superhero) -> Bool {
        return lhs.firstName == rhs.firstName &&
            lhs.lastName == rhs.lastName &&
            lhs.age == rhs.age
    }
}
```

You can define your own hash function to produce the value. We will not cover basis on hash but here is [a reference from Wikipedia](https://en.wikipedia.org/wiki/Hash_function). Hashable extends Equatable too which means we also need to conform to Equatable.

At last, let’s show before and after as usual.

Before:

```
// Error: Type 'Superhero' does not conform to protocol 'Hashable'
let superheroesSet: Set<Superhero> = [superman, batman]

// Error: Type 'Superhero' does not conform to protocol 'Hashable'
let superHeroDict: [Superhero: String]
```

After:
```

let superheroes = [superman, batman]
superheroes[0].firstName // “Clark”

let superHeroDict: [Superhero: String] = [superman: "Superman"]
superHeroDict[superman] // “Superman”
```

Thanks for reading this serial. More articles on Swift are coming.
