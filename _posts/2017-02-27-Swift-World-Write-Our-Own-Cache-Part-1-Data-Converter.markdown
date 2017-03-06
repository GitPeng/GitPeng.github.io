---
layout: post
title:  "Swift World: Write Our Own Cache Part 1: Data Converter"
date:   2017-02-27 20:00:00
categories: tutorial
---

While building apps,  we always need a cache. There’re already some excellent cache libraries from community like [Kingfisher](https://github.com/onevcat/Kingfisher), [HanekeSwift](https://github.com/Haneke/HanekeSwift) and [Cache](https://github.com/hyperoslo/Cache) . But in our own world, maybe we need a different cache according to specific requirements. Let’s learn to write our own cache in this series.

The first step to learn writing is reading. So we will read three cache libraries we listed. Kingfisher is specific for image and other two are generic for different types like String and JSON. We simplify the codes to make it more clear.

Normally, before being stored in cache, the instances of different types are all converted to Data. When reading back from cache, these data are converted back to previous type.

Simply speaking, there is a converter to make each type cachable.  For example,  [Cache](https://github.com/hyperoslo/Cache) library use protocol to define the main interface of this converter. The associated type is the type of original object which need to be converted to Data.

```swift
public protocol Cachable {
  associatedtype CacheType

  static func decode(_ data: Data) -> CacheType?

  func encode() -> Data?
}
```

For the cache framework for different types, every type extends the data converter protocol. Take UIImage as example, to make it Cachable, Cache library will encode it to Data according to if it has alpha channel and decode the data to UIImage  when being read back.

```swift
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

Kingfisher does the same thing even though it has not a specific protocol.

```swift
extension Kingfisher where Base: Image {
    public func pngRepresentation() -> Data? {
        ...
        return UIImagePNGRepresentation(base)
    }
}
```

```swift
static func image(data: Data, scale: CGFloat, preloadAllGIFData: Bool, onlyFirstFrame: Bool) -> Image? {
        var image: Image?

         switch data.kf.imageFormat {
         …
             case .PNG:
                    image = Image(data: data, scale: scale)
         }
         return image
}
```

We can find other extensions for String, NSDate and JSON to make them cachable.

Then as the first step to write our own cache, we will define converter protocol and make our customized type conform to it. It is a good method to reuse the current solution and enhance it according to our specific requirements. So we ruse the Cachable protocol in Cache library. For our own type Superhero, the definition and extension are as following.

```swift
struct Superhero {
    let firstName: String
    let lastName: String
}
```

```swift
extension Superhero: Cachable {
    typealias CacheType = Superhero

    public static func decode(_ data: Data) -> Superhero? {
        guard let fullName = String(data: data, encoding: String.Encoding.utf8) else {
            return nil
        }

        let fullNameArr = fullName.components(separatedBy: " ")
        let firstName    = fullNameArr[0]
        let lastName = fullNameArr[1]

        return Superhero(firstName: firstName, lastName: lastName)
    }

    public func encode() -> Data? {
        let fullName = firstName + " " + lastName
        return fullName.data(using: String.Encoding.utf8)
    }
}
```

That’ all for part 1. In next part, we will explain the cache interface and memory cache.

Thanks for your time.
