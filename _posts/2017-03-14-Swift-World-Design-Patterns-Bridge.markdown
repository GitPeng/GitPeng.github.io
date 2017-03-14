---
layout: post
title:  "Swift World: Design Patterns - Bridge"
date:   2017-03-14 20:00:00
categories: tutorial
---
Do you remember our system structure for car? We have a protocol and different implements like the code below.

```swift
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
```

Then what if we want to describe colored cars? This requirement add another variable in current structure. Normally, we need to add a inheritance level.

```swift
class RedSedan: Sedan {
    func drive() {
        print("drive a red sedan")
    }
}
```

The structure is in the below figure

![BridgeBefore](http://pengguo.xyz/resources/BridgeBefore.png)

Its disadvantage is too many inheritance levels. If we want to add new colors or new car types, the whole structure will becomes more complicated.

Let’s refactor with bridge pattern. The following figure depicts its structure.

> ![BridgePattern](https://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Bridge_UML_class_diagram.svg/1000px-Bridge_UML_class_diagram.svg.png)
> From [Bridge pattern - Wikipedia](https://en.wikipedia.org/wiki/Bridge_pattern)

```swift
protocol ColoredCar {
    var car: Car { get set }
    func drive()
}

class RedCar: ColoredCar {
    var car: Car

    init(car: Car) {
        self.car = car
    }

    func drive() {
        car.drive()
        print("It's red.")
    }
}

//usage
let sedan = Sedan()
let redSedan = RedCar(car: sedan)
redSedan.drive()
```

New structure is in the following figure.

![BridgeAfter](http://pengguo.xyz/resources/BridgeAfter.png)

Then there is no so many inheritances. The car and color will be scaled respectively.

Thanks for your time.
