---
layout: post
title:  "Swift World: Design Patterns - Singleton"
date:   2017-03-09 20:00:00
categories: tutorial
---
Singleton is very popular in Cocoa. We can find different use cases. The following are two examples.

```swift
let default = NotificationCenter.default
```

```swift
let standard = UserDefaults.standard
```

We will not talk about what singleton is and its advantages. You can find so many resources on the Internet. We want to focus on how to write our own?

Do you remember how to do this in the Objective-C world? Here is a template which maybe you’ve ever seen. The  `sharedInstance = [[Car alloc] init];` will only executed once. This is guaranteed by `dispatch_once`.

```objective-c
@interface Car : NSObject
@end

@implementation car

+ (instancetype)sharedInstance {
    static Car *sharedInstance = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        sharedInstance = [[Car alloc] init];
    });
    return sharedInstance;
}

@end
```

What about Swift?

```swift
class Car {
    static let sharedInstance = Car()
}
```

or a little complex

```swift
class Car {
    static let sharedInstance: Car = {
        let instance = Car()
        return instance
    }()
}
```

Really? It’s so simple. Then where is `dispatch_once`? How do you guarantee the codes only run once? The secret is the ‘static’ keywords. The static property will be  lazily initialized once and only once.

Thank you for your time.
