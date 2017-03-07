---
layout: post
title:  "Swift World: Design Patterns - Simple Factory"
date:   2017-03-07 20:00:00
categories: tutorial
---

If we want to learn a programming language, we need to live with it. It means to eat, drink, even breathe with it. It is very necessary to use Swift as much as possible. Today, we will start talking about design patterns in Swift. In addition to simple explanation, we also try to give figures, samples, use cases, etc. Let’s get started.

The first pattern is simple factory. Simply speaking, we build a factory to produce different objects which belong to the same type. As in the following figure and code, we have different types of cars, sedan, SUV, and maybe van. They all conform to protocol Car. It means the protocol defines common interface for cars. The specific car type implement their own logic.

![Before](http://pengguo.xyz/resources/SimpleFactoryPatternBefore.png)

```swift
enum CarType {
    case sedan, SUV
}

protocol Car {
    func drive()
}

class Sedan: Car {
    func drive() {
        print("drive a sedan")
    }
}

class SUV: Car {
    func drive() {
        print("drive a SUV")
    }
}

let sedan = Sedan()
sedan.drive()
let suv = SUV()
suv.drive()
```

If we want to add van, we create van class.

```swift
enum CarType {
    case sedan, SUV, van
}
```

```swift
class Van: Car {
    func drive() {
        print("drive a van")
    }
}
```

Then the instance

```swift
let van = Van()
van.drive()
```

Let’s try simple factory pattern to do the same thing. We need a factory to do the job to produce different types of cars.

![After](http://pengguo.xyz/resources/SimpleFactoryPatternAfter.png)

```swift
class Factory {
    static func produceCar(type: CarType) -> Car {
        switch type {
        case .sedan:
            return Sedan()
        case .SUV:
            return SUV()
        }
    }
}

let sedan = Factory.produceCar(type: .sedan)
sedan.drive()
let suv = Factory.produceCar(type: .SUV)
suv.drive()
```

Obviously, it is the factory which creates different instances of cars. Then what if we want to add van? The van class have been defined. So here we need to change the factory to enable it to produce van.

```swift
class Factory {
    static func produceCar(type: CarType) -> Car {
        switch type {
        case .sedan:
            return Sedan()
        case .SUV:
            return SUV()
        case .van
            return Van()
        }
    }
}

let sedan = Factory.produceCar(type: .sedan)
sedan.drive()
let suv = Factory.produceCar(type: .SUV)
suv.drive()
let van = Factory.produceCar(type: .van)
van.drive()
```

That’s all simple factory pattern does. But why do we need this pattern? It separates the creation from usage and move the responsibility  to a specific role. Or speak in professional terms, it help loose coupling. Let’s assume the initialization logic of sedan is changed one day. We don’t need to change every creation in the whole project. To change the logic in factory is all we need to do.

In next blog of this series, we will talk about Factory Method Pattern.

Thanks for your time.
