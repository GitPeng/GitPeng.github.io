---
layout: post
title:  "Swift World: Design Patterns - Proxy"
date:   2017-03-17 20:00:00
categories: tutorial
---

Today, we will talk about proxy pattern. In this pattern, proxy is an object to help us to access another object. It simply delegates real job to that object or change its behavior. The following figure depicts the roles and their relationships.

> ![Proxy](https://upload.wikimedia.org/wikipedia/commons/thumb/7/75/Proxy_pattern_diagram.svg/800px-Proxy_pattern_diagram.svg.png)
> from [Proxy pattern - Wikipedia](https://en.wikipedia.org/wiki/Proxy_pattern)

Proxy pattern is popularly used in Cocoa which even has a specific NSProxy class in it. Another example is UIApperance protocol and other relevant types.

We will continue using our car system.

```swift
protocol Car {
    func drive()
}

class Sedan: Car {
    func drive() {
        print("drive a sedan")
    }
}
```

Autonomous car is so hot topic now. So let’s build our own. Actually, it’s not built from scratch but enhance our current car with self-driving system. So it has a internal car instance and delegate the driving to the car. But as an autonomous car, it controls the car automatically. Delegation and change are what a proxy does in this pattern.

```swift
class AutonomousCar: Car {
    var car: Car

    init(car: Car) {
        self.car = car
    }

    func drive() {
        car.drive()
        print("by self-driving system")
    }
}
```

Let’s see how to drive it.

```swift
//usage
let sedan = Sedan()
let autonomousCar = AutonomousCar(car: sedan)
autonomousCar.drive()
```

Thanks for your time.
