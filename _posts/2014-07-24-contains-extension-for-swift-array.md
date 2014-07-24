---
layout: post
title: Contains Extension for Swfit Array
---
This post is from my answer to the stackoverflow question [Swift's equatable protocol conformance check](http://stackoverflow.com/questions/24174697/swifts-equatable-protocol-conformance-check). Actually the poster wanted to extend Swift's array to get kind of containment verification. I think the [ExSwift](https://github.com/pNre/ExSwift)'s solution is better. So i list it here.

    func contains <T: Equatable> (items: T...) -> Bool
    {
        return items.all { self.indexOf($0) >= 0 }
    }

    func indexOf <U: Equatable> (item: U) -> Int?
    {
        if item is Element
        {
            if let found = find(reinterpretCast(self) as   Array<U>, item)
            {
                return found
            }

            return nil
        }

        return nil
    }

    func all (call: (Element) -> Bool) -> Bool
    {
        for item in self
        {
            if !call(item)
            {
                return false
            }
        }

        return true
    }
