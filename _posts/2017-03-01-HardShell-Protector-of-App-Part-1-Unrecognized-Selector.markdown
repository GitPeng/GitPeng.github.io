---
layout: post
title:  "HardShell - Protector of App Part 1: Unrecognized Selector"
date:   2017-03-01 20:00:00
categories: tutorial
---
[HardShell](https://github.com/NilStack/HardShell) is a protector to keep app from some specific crashes to keep it running smoothly. As step one, it only protects app from the crash caused by "unrecognized selector sent to instance". More features will come step by step. It’s our first project with Objective-C runtime.

The idea is from [Baymax's introduction](https://neyoufan.github.io/2017/01/13/ios/BayMax_HTSafetyGuard/) which is from Wangyi (Hangzhou). They promise Baymax SDK will be released but don't mention the schedule and if it will be opensourced. So We build HardShell.

### Why

While building apps, unrecognized selector is a very common cause to crash. Hotfix is not easy for iOS app. So keep the app running smoothly even there is “unrecognized selector sent to instance”.

### How

[HardShell](https://github.com/NilStack/HardShell)  leverages [Message Forwarding](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtForwarding.html) in Objective-C. We will not explain the details of this mechanism, please refer to the official document. In HardShell, we rewrite `forwardingTargetForSelector:` in NSObject extension. Simply speaking, we forward the selector to a stub class we create at runtime dynamically. Talk is cheap, let’s read the code.
Please pay attention, we only verify the idea with current code and have not covered edge cases.

1. Create a stub class with `objc_allocateClassPair:`.

```
NSString *stubClassName = @"HardShellStub";
    Class stubClass = NSClassFromString(stubClassName);
    if(stubClass == nil){
        stubClass = objc_allocateClassPair([NSObject class], [stubClassName UTF8String], 0);
    }
```

2. Create a method for the unrecognized selector

```
IMP imp = imp_implementationWithBlock(^(__unsafe_unretained id self, va_list argp) {
        return 0;

    });
```

3. Add method to stub class

```
class_addMethod(stubClass, aSelector, imp, "v@:");
```

4. Return instance of the stub class

```
return [[stubClass alloc] init];
```

### Questions:

We will try to figure out two problems. One is what we should do if `forwardInvocation:` is rewritten by original class? The other one is what if class_addMethod fails? Please leave your comments if possible to help us improve the solution.
