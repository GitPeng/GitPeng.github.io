---
layout: post
title:  "Swift World: Write Our Own Cache Part 3 - Memory Cache and Disk Cache"
date:   2017-02-28 20:00:00
categories: tutorial
---

In this part, we will talk about memory cache and disk cache. The strategy in popular cache frameworks are combination of memory cache and disk cache to leverage each one’s advantages.

NSCache is often used as memory Cache. For example in Kingfisher,

```
open class ImageCache {
    //Memory
    fileprivate let memoryCache = NSCache<NSString, AnyObject>()
    ...
}
```

Write image to memory cache:

```
memoryCache.setObject(image, forKey: computedKey as NSString, cost: image.kf.imageCost)
```

Retrieve image from memory cache with key:

```
open func retrieveImageInMemoryCache(forKey key: String, options: KingfisherOptionsInfo? = nil) -> Image? {

        let options = options ?? KingfisherEmptyOptionsInfo
        let computedKey = key.computedKey(with: options.processor.identifier)

        return memoryCache.object(forKey: computedKey as NSString) as? Image
    }
```

It isn’t difficult to understand the logic. We will go on to disk cache.

Disk cache uses iOS’s file system to store data converted from object. Usually it will create its own directory in app’s caches directory. A file will be created for every object. For example in [Cache](https://github.com/hyperoslo/Cache)

```
let paths = NSSearchPathForDirectoriesInDomains(.cachesDirectory,
      FileManager.SearchPathDomainMask.userDomainMask, true)
path = "\(paths.first!)/\(DiskStorage.prefix).\(cacheName)"
```

```
 ...
 try weakSelf.fileManager.createDirectory(atPath: weakSelf.path,
            withIntermediateDirectories: true, attributes: nil)
...
weakSelf.fileManager.createFile(atPath: filePath,
          contents: object.encode() as Data?, attributes: nil)

```

In Kingfisher

```
public final class func defaultDiskCachePathClosure(path: String?, cacheName: String) -> String {
        let dstPath = path ?? NSSearchPathForDirectoriesInDomains(.cachesDirectory, .userDomainMask, true).first!
        return (dstPath as NSString).appendingPathComponent(cacheName)
    }
```

```
...
try self.fileManager.createDirectory(atPath: self.diskCachePath, withIntermediateDirectories: true, attributes: nil)
...
self.fileManager.createFile(atPath:self.cachePath(forComputedKey: computedKey), contents: data, attributes: nil)
...
```

If these IO operations on file system run in main thread, the UI will be stuck because reading from and writing to disk are time consuming. The popular method is to create new DispatchQueues to do the job asynchronously.

For example in [Cache](https://github.com/hyperoslo/Cache)

```
  /// Queue for write operations
  public fileprivate(set) var writeQueue: DispatchQueue
  /// Queue for read operations
  public fileprivate(set) var readQueue: DispatchQueue
  ...
  writeQueue = DispatchQueue(label: "\(DiskStorage.prefix).\(cacheName).WriteQueue",
      attributes: [])
    readQueue = DispatchQueue(label: "\(DiskStorage.prefix).\(cacheName).ReadQueue",
      attributes: [])
```

In next part, we will introduce cache manager which control the complete workflow.

Thanks for your time.
