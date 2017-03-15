---
layout: post
title:  "Swift World: Design Patterns - Decorator"
date:   2017-03-15 20:00:00
categories: tutorial
---

Decorator is a structural pattern to add new functions to class or instance at runtime. Compared to inheritance, it has more advantages in flexibility and scalability.  We will still use our car model in this article.

```swift
enum CarType: String {
    case Sedan = "sedan"
    case SUV = "SUV"
}

protocol Car {
    var type: CarType{ get }
    func drive()
}

class Sedan: Car {
    var type: CarType = .Sedan

    func drive() {
        print("drive a " + type.rawValue)
    }
}
```

Now  we want to make an autonomous sedan. Normally, we will create a new class whose super class is Sedan and add new feature.

```swift
class AutonomousSedan: Sedan {
    override func drive() {
        print("automatically drive a " + type.rawValue)
    }
}
```

It’s ok because we get what we want finally. But what if we want to add more   features? What if we want to make other car autonomous? The inheritance will make the whole structure more and more complicated. Let’s see if decorator pattern can help us.

This figure below depicts the basic structure of decorator pattern. We will create a new role decorator which inherits car and has an instance of car,  and its subclass for specific feature.  

> ![DecoratorPattern](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e9/Decorator_UML_class_diagram.svg/800px-Decorator_UML_class_diagram.svg.png)
> From [Decorator pattern - Wikipedia](https://en.wikipedia.org/wiki/Decorator_pattern)

```swift
class Decorator: Car {
    var car: Car
    var type: CarType {
        return car.type
    }

    init(car: Car) {
        self.car = car
    }
    func drive() {
        car.drive()
    }
}

class AutonomousCar: Decorator {
    override func drive() {
        print("automatically drive a " + type.rawValue)
    }
}
```

Let’s see how to use it.

```swift
let sedan = Sedan()
let autonomousSedan = AutonomousCar(car: sedan)
autonomousSedan.drive()
```

We add autonomous function to sedan without inheritance.  If we want to add other new feature, create another subclass of decorator. And if we want to add autonomous function to other car, make an instance and pass it to AutonomousCar’s initializer. The whole structure is clear and scalable.

Please try to write the codes for the logic we described. If possible, please solve same questions with inheritance. You will get direct experience by comparison.

Thanks for your time.
