---
layout: post
title:  "Swift World: Design Patterns - Factory Method"
date:   2017-03-08 20:00:00
categories: tutorial
---

Do you remember the Simple Factory Pattern we talked about in last article?

![After](http://pengguo.xyz/resources/SimpleFactoryPatternAfter.png)

As the figure tells us, we build a factory to produce different types of cars. But what if the list becomes too long. Now, we have truck, sports car, bus, etc. All the cars will be produced in just one factory. It will make the factory stressful. So we will introduce another pattern Factory Method to help it.

Since the factory feels stressful to produce so many kinds of car, why not build more factories?  Good idea,  that’s what we will do with Factory Method pattern. We’re going to build a factory for each type to make them  stay focused. These factories have same interface because they all produce cars.

![FactoryMethodSmall](http://pengguo.xyz/resources/FactoryMethodSmall.png)

Then let’s change our code.

```swift
/// abstract factory
protocol Factory {
    func produce() -> Car
}

/// concrete factory
class SedanFactory: Factory {
    func produce() -> Car{
        return Sedan()
    }
}

/// concrete factory
class SUVFactory: Factory {
    func produce() -> Car {
        return SUV()
    }
}

let sedanFactory = SedanFactory()
let sedan = sedanFactory.produce()
sedan.drive()

let suvFactory = SUVFactory()
let suv = suvFactory.produce()
suv.drive()
```

Factory Method pattern makes the codebase more flexible to add or remove new types.
To add a new type, we just need a new class for the type and a new factory to produce it like the following code. We don’t need to change original codebase.

```swift
class Truck: Car {
    func drive() {
        print("drive a truck")
    }
}

class TruckFactory: Factory {
    func produce() -> Car {
        return Truck()
    }
}

let truckFactory = TruckFactory()
let truck = truckFactory.produce()
truck.drive()
```

That’s all for Factory Method Pattern.

Thanks for your time.
