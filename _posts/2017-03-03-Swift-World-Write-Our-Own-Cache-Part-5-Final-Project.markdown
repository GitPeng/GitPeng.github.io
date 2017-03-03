---
layout: post
title:  "Swift World: Write Our Own Cache Part 5 - Final Project"
date:   2017-03-03 20:00:00
categories: tutorial
---

After reading and analyzing popular cache frameworks, let’s start our own. In previous parts, we get main components including data converter, cache protocol, memory cache, disk cache and cache manager. First let’s give our own cache framework a name. We know naming is the most difficult in programming. How about Hoard? All the codes will be pushed to [GitHub - NilStack/Hoard](https://github.com/NilStack/Hoard).  Please pay attention there’re many compromises for simplicity. It’s not a product ready framework.

#### Data converter
In this section, we build data converter protocol and extension for different types. We prefer [Cache](https://github.com/hyperoslo/Cache)’s design on this part.

Cachable.swift

```
public protocol Cachable {
    associatedtype CacheType

    static func decode(_ data: Data) -> CacheType?

    func encode() -> Data?
}
```

Then we need to extend the type we want to cache to conform the protocol.

UIImage+Cache.swift

```
extension UIImage: Cachable {
    public typealias CacheType = UIImage

    public static func decode(_ data: Data) -> CacheType? {
        let image = UIImage(data: data)
        return image
    }

    public func encode() -> Data? {
        return hasAlpha
            ? UIImagePNGRepresentation(self)
            : UIImageJPEGRepresentation(self, 1.0)
    }
}
```

#### Cache protocol

Cache protocol defines common interfaces to manage objects in cache. We only define the least requirements for a cache to store, retrieve and remove objects.

```
public protocol Cache {

    func store<T: Cachable>(key: String, object: T, completion: (() -> Void)?)

    func retrieve<T: Cachable>(_ key: String, completion: @escaping (_ object: T?) -> Void)

    func remove(_ key: String, completion: (() -> Void)?)

}
```

#### Memory Cache

Memory cache can simply be taken as wrapper for NSCache, it only delegates all jobs to NSCache.

MemoryCache.swift

```
class MemoryCache: Cache {
    public let cache = NSCache<AnyObject, AnyObject>()

    func store<T : Cachable>(key: String, object: T, completion: (() -> Void)?) {
        cache.setObject(object as AnyObject, forKey: key as AnyObject)
        completion?()
    }

    func retrieve<T : Cachable>(_ key: String, completion: @escaping (T?) -> Void) {
        let object = cache.object(forKey: key as AnyObject) //as? Capsule
        completion(object as? T)
    }

    func remove(_ key: String, completion: (() -> Void)?) {
        cache.removeObject(forKey: key as AnyObject)
        completion?()
    }
}
```

#### Disk Cache

It’s a little complex for disk cache but basically they’re file operations like creating file, reading file and removing file. Please refer to DiskCache.swift in [GitHub - NilStack/Hoard](https://github.com/NilStack/Hoard).

#### Cache Manager
It controls the complete workflow and coordinate memory cache and disk cache.

```
class Hoard: Cache {
    static let sharedCache = Hoard()

    let memoryCache = MemoryCache()
    let diskCache = DiskCache()

    public func store<T : Cachable>(key: String, object: T, completion: (() -> Void)?) {
    }
    ...
}
```


#### Usage

Here is example code to use our small cache framework.

```
let imageURL = URL(string: "https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png")!
        let image = try! UIImage(data: Data(contentsOf: imageURL))

        let cache = Hoard.sharedCache
        cache.store(key: "image", object: image!, completion: nil)
        cache.retrieve(key: "image") { (image: UIImage?) in
            if let image = image {
                imageView.image = image
            } else {
                print("no image")
            }
        }
```

You can find the complete codebase at [GitHub - NilStack/Hoard](https://github.com/NilStack/Hoard). Hoard is only demo for this series and not product ready.

Thanks for your time.
