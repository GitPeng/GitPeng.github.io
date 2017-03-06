---
layout: post
title:  "Swift World: Write Our Own Cache Part 4 - Cache Manager"
date:   2017-03-02 20:00:00
categories: tutorial
---

Although given different names, there is always a management role to control the complete workflow. It will coordinate the memory cache and disk cache we talked about in last part.

Like the ImageCache class in [Kingfisher](https://github.com/onevcat/Kingfisher), it has both memory cache and disk cache instance. More important, ImageCache control the complete logic to read and write object between two cache types. Let’s start reading the code.  We only show the main logic to make it more clear. Please read the source code if you want more details.

#### Memory cache and disk cache

```swift
open class ImageCache {
    //Memory Cache
    fileprivate let memoryCache = NSCache<NSString, AnyObject>()
    ...
    ///The disk cache location.
    open let diskCachePath: String
    ...
}
```

#### Disk IO operation’s DispatchQueue

```swift
let ioQueueName = "com.onevcat.Kingfisher.ImageCache.ioQueue.\(name)"
ioQueue = DispatchQueue(label: ioQueueName)
```

#### FileManager to manipulate file system for disk cache

```swift
fileprivate var fileManager: FileManager!
ioQueue.sync { fileManager = FileManager() }
```

### Logic to write

We use ellipses to simplify the parameter list. First, ImageCache write the image to memoryCache with a key in following codes.

```swift
// We use ... to simplify the parameter list
open func store(…) {
    let computedKey = key.computedKey(with: identifier)
    memoryCache.setObject(image, forKey: computedKey as NSString, cost: image.kf.imageCost)
    ...
}
```

Then create new file for the image data in ioQueue.

```swift
ioQueue.async {
    do {
        try self.fileManager.createDirectory(atPath: self.diskCachePath, withIntermediateDirectories: true, attributes: nil)
        } catch _ {}
    ...
    self.fileManager.createFile(atPath: self.cachePath(forComputedKey: computedKey), contents: data, attributes: nil)
}
```

#### Logic to retrieve
Util now, the image is cached in memory and disk. Then let’s see the logic to retrieve the image from the cache. The image will be retrieved in memory cache first with NSCache’s `object(forKey key: KeyType) -> ObjectType?` .  If the image is not there, disk cache will be next. Then if it is in disk cache,  the image will be read and stored in memory cache. At last, the image data will be cached in both memory and disk. Next time, it becomes fast to retrieve the image from memory instead of disk.

```swift
open func retrieveImage(forKey key: String, ...) {
     if let image = self.retrieveImageInMemoryCache(forKey: key, options: options) {
         ...
     } else {
         if let image = sSelf.retrieveImageInDiskCache(forKey: key, options: options) {
              ...
              sSelf.store(...)
              ...
          }
     }
}
```

If the image is not in either memory cache or disk cache, it will be fetched from network or other resources. It’s your own job.

The main logic is familiar in other cache frameworks. You can refer to their codebase.

Until now, we have talked about the core components and main logic for a cache framework. We will start building our own in next article.
