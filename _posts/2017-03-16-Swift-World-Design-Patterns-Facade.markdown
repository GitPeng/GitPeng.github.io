---
layout: post
title:  "Swift World: Design Patterns - Facade"
date:   2017-03-16 20:00:00
categories: tutorial
---

Literally, facade means
> the face of a building, especially the principal front that looks onto a street or open space.
> from [Facade - Google Search](https://www.google.com/search?q=Facade)

Similarly, as design pattern facade defines an simpler interface to an complex subsystem. For example, in our car factory, we have different departments to produce different components like engine, body, and accessories. As client, we don’t care how every department does produce its own job. We just create a factory instance and get it to work.

```swift
class Engine {
    func produceEngine() {
        print("prodce engine")
    }
}

class Body {
    func produceBody() {
        print("prodce body")
    }
}

class Accessories {
    func produceAccessories() {
        print("prodce accessories")
    }
}
```

So we build a facade to provide a simple interface.

```swift
class FactoryFacade {
    let engine = Engine()
    let body = Body()
    let accessories = Accessories()

    func produceCar() {
        engine.produceEngine()
        body.produceBody()
        accessories.produceAccessories()
    }
}
```

Then use the factory directly.

```swift
let factoryFacade = FactoryFacade()
factoryFacade.produceCar()
```

The structure figure is clear.

![Facade](http://pengguo.xyz/resources/Facade.png)

Thanks for your time.
