---
layout: post
title:  "Swift World: Design Patterns - Flyweight"
date:   2017-03-18 20:00:00
categories: tutorial
---
Flyweight is about sharing. It holds a pool to store objects. The client will reuse existing objects in the pool.
In previous articles, we have a general interface for car and a specific sedan class as following. Each car

```swift
protocol Car {
    var color: UIColor { get }
    func drive()
}

class Sedan: Car {
    var color: UIColor

    init(color: UIColor) {
        self.color = color
    }

    func drive() {
        print("drive a sedan")
    }
}
```

we also have built our factory which produces sedan. Generally, we have sedan in different colors in stock. When there are orders, we look up in our stock first and reuse it.

```swift
class Factory {
    var cars: [UIColor: Car] = [UIColor: Car]()

    func getCar(color: UIColor) -> Car {
        if let car = cars[color] {
            return car
        } else {
            let car = Sedan(color: color)
            cars[color] = car
            return car
        }
    }
}
```

Let’s get cars from factory.

```swift
let factory = Factory()
let redSedan = factory.getCar(color: .red)
redSedan.drive()
```

Util now, we’ve completed structural patterns. If you want to learn others, we list them here.

* [Swift World: Design Patterns - Adapter](http://pengguo.xyz/tutorial/2017/03/13/Swift-World-Design-Patterns-Adapter.html)
* [Swift World: Design Patterns - Bridge](http://pengguo.xyz/tutorial/2017/03/14/Swift-World-Design-Patterns-Bridge.html)
* [Swift World: Design Patterns - Decorator](http://pengguo.xyz/tutorial/2017/03/15/Swift-World-Design-Patterns-Decorator.html)
* [Swift World: Design Patterns - Facade](http://pengguo.xyz/tutorial/2017/03/16/Swift-World-Design-Patterns-Facade.html)
* [Swift World: Design Patterns - Proxy](http://pengguo.xyz/tutorial/2017/03/17/Swift-World-Design-Patterns-Proxy.html)

 Thanks for your time.
