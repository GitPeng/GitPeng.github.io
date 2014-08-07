---
layout: post
title: Singleton in Swift
---

Singleton is a popular pattern in Object-C. When moving to Swift, what's the best practice in a so different world. @hpique listed three approachs in [SwiftSingleton](https://github.com/hpique/SwiftSingleton): global constant, nested struct and dispatch_once. He recommended the second one. In @mattt's new project [Alamofire](https://github.com/Alamofire/Alamofire), he also implement the singlton in the same approach.

     class Manager {
        class var sharedInstance: Manager {
            struct Singleton {
                static let instance = Manager()
            }
            return Singleton.instance
        }
        ...
    }


  I will test this pattern in my coming project.
