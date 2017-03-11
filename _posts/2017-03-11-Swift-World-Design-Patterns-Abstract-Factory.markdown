---
layout: post
title:  "Swift World: Design Patterns - Abstract Factory"
date:   2017-03-11 20:00:00
categories: tutorial
---

Today we will talk about abstract factory pattern. It handles a little more complex use case. As we know, the sedan family has different models like compact, mid-size and full-size.SUV has same categories. Let’s assume we have two factories. One is focused on compact types and the other is on full size. As the following figure tells us, factory A produces compact sedan and compact SUV. Factory B produces full-size sedan and SUV.


![AbstractFactory](http://pengguo.xyz/resources/AbstractFactory.png)

Let’s start coding. Here is different sizes of sedans and SUVs. They all conform to respective abstract interface.  

```swift
protocol Sedan {
    func drive()
}

class CompactSedan: Sedan {
    func drive() {
        print("drive a compact sedan")
    }
}

class MidSizeSedan: Sedan {
    func drive() {
        print("drive a mid-size sedan")
    }
}

class FullSizeSedan: Sedan {
    func drive() {
        print("drive a full-size sedan")
    }
}
```


```swift
protocol SUV {
    func drive()
}

class CompactSUV: SUV {
    func drive() {
        print("drive a compact SUV")
    }
}

class MidSizeSUV: SUV {
    func drive() {
        print("drive a mid-size SUV")
    }
}

class FullSizeSUV: SUV {
    func drive() {
        print("drive a full-size SUV")
    }
}
```

Then the following is our factories. They all can produce sedan and SUV. A is   for compact and B is for full-size.

```swift
protocol Factory {
    func produceSedan() -> Sedan
    func produceSUV() -> SUV
}

class FactoryA: Factory {
    func produceSedan() -> Sedan{
        return CompactSedan()
    }

    func produceSUV() -> SUV {
        return CompactSUV()
    }
}

class FactoryB: Factory {
    func produceSedan() -> Sedan {
        return FullSizeSedan()
    }

    func produceSUV() -> SUV {
        return FullSizeSUV()
    }
}
```

Ok, let’s produce sedan and SUV now.

```
let factoryA = FactoryA()
let compactSedan = factoryA.produceSedan()
let compactSUV = factoryA.produceSUV()
compactSedan.drive()
compactSUV.drive()

let factoryB = FactoryB()
let fullsizeSedan = factoryB.produceSedan()
let fullsizeSUV = factoryB.produceSUV()
fullsizeSedan.drive()
fullsizeSUV.drive()
```

Another use case is to make different themes for our UI. There are many elements involved in a theme like label, button, background, etc. But in specific theme, each has a style. Please try abstract factory to build this modal.

If you want to learn other creational patterns, we list them here.

[Swift World: Design Patterns - Simple Factory](http://pengguo.xyz/tutorial/2017/03/07/Swift-World-Design-Patterns-Simple-Factory.html)
[Swift World: Design Patterns - Factory Method](http://pengguo.xyz/tutorial/2017/03/08/Swift-World-Design-Patterns-Factory-Method.html)
[Swift World: Design Patterns - Singleton](http://pengguo.xyz/tutorial/2017/03/09/Swift-World-Design-Patterns-Singleton.html)
[Swift World: Design Patterns - Builder](http://pengguo.xyz/tutorial/2017/03/10/Swift-World-Design-Patterns-Builder.html)

Thanks for your time.
