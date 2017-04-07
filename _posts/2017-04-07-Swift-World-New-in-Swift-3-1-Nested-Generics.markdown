---
layout: post
title:  "Swift World: New in Swift 3.1 - Nested Generics"
date:   2017-04-07 20:00:00
categories: tutorial
---

Both nested types and generics are great features of Swift. Today, we will introduce nested generics which mixes them. It reminds me of  [Pen-Pineapple-Apple-Pen](https://www.youtube.com/watch?v=0E00Zuayv9Q). :)

```
We have nested type, we have generics.
Uh! Nested Generics
```

Let’s get started with a real world scenario.

```swift
public class LinkedListNode<T> {
    var value: T
    var next: LinkedListNode?
    weak var previous: LinkedListNode?

    public init(value: T) {
        self.value = value
    }
}

public final class LinkedList<T> {
    public typealias Node = LinkedListNode<T>

    fileprivate var head: Node?

    public init() {}

    ...
}
```

This code snippet is to build a linked list  which is from [Swift algorithm club](https://github.com/raywenderlich/swift-algorithm-club).

In this example, the node class is only for linked list, so nested type is good choice to improve the encapsulation.   

```swift
public final class LinkedList<T> {
    class Node<T> {
        var value: T
        var next: Node?
        weak var previous: Node?

        public init(value: T) {
            self.value = value
        }
    }

    fileprivate var head: Node<T>?

    public init() {}

    ...
}
```

The T for Node class is inheriting from the outer LinkedList. It means a linked list for Int has the node which encapsulates Int value. We can even remove the generic declaration for Node like following codes.

```swift
class Node {
    var value: T
    var next: Node?
    weak var previous: Node?

    public init(value: T) {
        self.value = value
    }
}
```

Thanks for your time.
