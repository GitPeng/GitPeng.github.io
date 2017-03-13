---
layout: post
title:  "Swift World: Design Patterns - Adapter"
date:   2017-03-13 20:00:00
categories: tutorial
---

We have finished creational patterns and will introduce structural patterns from this article.  Literally, structural patterns are about structure. It means how to organize classes or instances to form larger structures. The first we will talk about is adapter pattern.

The latest adapter example in real world is lightning to 3.5 mm headphone Jack adapter. If we want to reuse old headphone with our new iPhone 7 which uses lightning connector, we need this adapter to connect them.

![HeadphoneAdapter](http://pengguo.xyz/resources/HeadphoneAdapter.png

Obviously, the headphone jack adapter is adapter. The old 3.5 mm headphone is adaptee. In the programming world, the adaptee is old class we want to reuse. But its interface is not compatible with new interface. So we need an adapter to help them.

We can implement adapter pattern in two ways. The first one is object adapter which uses composition. There is an adaptee instance in adapter to do the job like the following figure and code tell us.

![ObjectAdapter](http://pengguo.xyz/resources/ObjectAdapter.png)

```swift
protocol Target {
    func request()
}

class Adapter: Target {
    var adaptee: Adaptee

    init(adaptee: Adaptee) {
        self.adaptee = adaptee
    }

    func request() {
        adaptee.specificRequest()
    }
}

class Adaptee {
    func specificRequest() {
        print("Specific request")
    }
}

// usage
let adaptee = Adaptee()
let tar = Adapter(adaptee: adaptee)
tar.request()
```

The other one is class adapter which uses multiple inheritance to connect target and adaptee.

![ClassAdapter](http://pengguo.xyz/resources/ClassAdapter.png)

```
protocol Target {
    func request()
}

class Adaptee {
    func specificRequest() {
        print("Specific request")
    }
}

class Adapter: Adaptee, Target {s
    func request() {
        specificRequest()
    }
}

// usage
let tar = Adapter()
tar.request()
```

This is all about the theory. In next articles,   we will bring more examples in real programming world.

Thanks for your time.
