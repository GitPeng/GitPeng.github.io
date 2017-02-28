---
layout: post
title:  "Swift World: Write Our Own Cache Part 2 - Cache Protocol"
date:   2017-02-28 20:00:00
categories: tutorial
---

In the previous part, we talked about data converter to make the object cachable. Today, we will build abstract cache. The popular cache libraries always include two layers, one is memory cache and the other is disk cache. The memory cache’s advantage is fast and the disk cache’s is capacity. So the general strategy is taking memory cache as front end and disk one as backend. Simply speaking, it means we will first look up the target in memory cache, if it isn’t there, then we will go on to disk cache.  

For example in Kingfisher

```
open class ImageCache {
    //Memory
    fileprivate let memoryCache = NSCache<NSString, AnyObject>()
    ...
    ///The disk cache location.
    open let diskCachePath: String
    ...
}
```

There is a little complicated in Cache library but the core idea is same.

```
public final class MemoryStorage: StorageAware {
     ...
     /// Memory cache instance
     public let cache = NSCache<AnyObject, AnyObject>()
     ...
}
```

```
public final class DiskStorage: StorageAware {
    ...
    /// Storage root path
    public let path: String
    /// Maximum size of the cache storage
    public var maxSize: UInt
    ...
}
```

```
public class BasicHybridCache: NSObject {
   ...
  /// Front cache (should be less time and memory consuming)
  let frontStorage: StorageAware
  // BAck cache (used for content that outlives the application life-cycle)
  var backStorage: StorageAware
  ...
}
```

We will talk about memory cache and disk cache in next parts.

Next, we will see the abstract cache with protocol.  Why do we need cache protocol? The advantage is to improve scalability.

![CacheProtocol](http://pengguo.xyz/resources/cacheprotocol.png)

As a cache, the client will write object to it and read from it when necessary. So in the following code in [Haneke](https://github.com/Haneke/HanekeSwift)

```
public class Cache<T: DataConvertible where T.Result == T, T : DataRepresentable> {
    public func set(value value: T, key: String, formatName: String = HanekeGlobals.Cache.OriginalFormatName, success succeed: ((T) -> ())? = nil)  {
        ...
    }
    public func fetch(key key: String, formatName: String = HanekeGlobals.Cache.OriginalFormatName, failure fail : Fetch<T>.Failer? = nil, success succeed : Fetch<T>.Succeeder? = nil) -> Fetch<T> {
        ...
    }
}
```

And in [Cache](https://github.com/hyperoslo/Cache)

```
public class BasicHybridCache: NSObject {
    public func add<T: Cachable>(_ key: String, object: T, expiry: Expiry? = nil, completion: (() -> Void)? = nil) {
        ...
    }

    public func object<T: Cachable>(_ key: String, completion: @escaping (_ object: T?) -> Void) {
        ...    
    }
}
```

Now, it’s not difficult to write your own cache protocol. Except these two basic features, we also want to remove objects anytime we want to or when they’re expired. So you can add functions to meet your own requirements.

In next blog, we will talk about memory cache.

Thanks for your time.
