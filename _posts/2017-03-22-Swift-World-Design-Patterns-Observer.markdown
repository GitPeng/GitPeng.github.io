---
layout: post
title:  "Swift World: Design Patterns - Observer"
date:   2017-03-22 20:00:00
categories: tutorial
---

While developing iOS apps, maybe you will use Key-Value Observing (KVO) which helps one object observe another’s state changes. It’s an example of observer pattern. If you want to learn more about it, the [official programming guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html) and [this article](https://www.objc.io/issues/7-foundation/key-value-coding-and-observing/) from objc.io  are good references.

As the following figure tells us, there are subject and observer in this pattern. The subject is the target to be observed. It holds the observer instances and will notify them about the changes. The observers get notified and handle the change. The logic is clear.

![ObserverPattern](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8d/Observer.svg/1000px-Observer.svg.png)

Let’s writing the codes. As an observer, it will only print some information when it is notified.

```swift
protocol Observer {
    func update()
}

class ConcreteObserver: Observer {
    var id: String

    init(id: String) {
        self.id = id
    }

    func update() {
        print(id + " observered the subject's state was changed.")
    }
}
```

As we mentioned, the subject has an observer list and notify every observer when its state is changed.

```swift
class Subject {
    var observers: [Observer] = []
    var state: String {
        didSet {
            notify()
        }
    }

    init(state: String) {
        self.state = state
    }

    func attach(observer: Observer) {
        observers.append(observer)
    }

    func notify() {
        for observer in observers {
            observer.update()
        }
    }
}
```

Let’s see how to use it.

```swift
//usage
let subject = Subject(state: "initial state")
let observerA = ConcreteObserver(id: "A")
let observerB = ConcreteObserver(id: "B")
subject.attach(observer: observerA)
subject.attach(observer: observerB)
subject.state = "new state"
```

That’s all for today. Thanks for your time.
