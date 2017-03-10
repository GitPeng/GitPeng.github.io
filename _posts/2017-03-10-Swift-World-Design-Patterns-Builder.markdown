---
layout: post
title:  "Swift World: Design Patterns - Builder"
date:   2017-03-10 20:00:00
categories: tutorial
---

Normally,  while building a car, we build every part first and then assemble them up. As customers, we don’t need to know how to produce every component. It’s the producer’s job to produce a functional car according to our requirements.

Do you remember our factory in [Factory Method Pattern](http://pengguo.xyz/tutorial/2017/03/08/Swift-World-Design-Patterns-Factory-Method.html) ?

```swift
protocol Factory {
    func produce() -> Car
}
```

 It’s too simple to describe the real world. Let’s enhance it.

```swift
protocol Factory {
    func produceWheel()
    func produceEngine()
    func produceChassis()
}
```

 We still need concrete factory to produce the real car for example a sedan factory or a SUV factory.

```swift
class SedanFactory: Factory {
    func produceWheel() {
        print("produce wheel for sedan")
    }
    func produceEngine() {
        print("produce engine for sedan")
    }
    func produceChassis() {
        print("produce chassis for sedan")
    }
}
```

```swift
class SUVFactory: Factory {
    func produceWheel() {
        print("produce wheel for SUV")
    }
    func produceEngine() {
        print("produce engine for SUV")
    }
    func produceChassis() {
        print("produce chassis for SUV")
    }
}
```

Now, we can produce sedan and SUV. But as customers, we don’t give order to the factory directly. Let’s assume there is ‘director’ between customers and automaker. The director gets orders from customers and direct factories to produce cars.

```swift
class Director {
    var factory: Factory

    init(factory: Factory) {
        self.factory = factory
    }

    func produce() {
        factory.produceWheel()
        factory.produceEngine()
        factory.produceChassis()
    }
}
```

Let’s start to produce cars.

```swift
let sedanFactory = SedanFactory()
let suvFactory = SUVFactory()

let sedanDirector = Director(factory: sedanFactory)
sedanDirector.produce()
let suvDirector = Director(factory: suvFactory)
suvDirector.produce()
```

So the structure becomes to the following.

![Builder](http://pengguo.xyz/resources/Builder.png)

Thanks for your time.
