---
layout: post
title:  "Swifty Leetcode: No.1 Two Sum"
date:   2017-03-03 20:00:00
categories: tutorial
---

The only way to learn Swift is practicing. In addition to starting an app or framework,  to solve problems in Swifts is very helpful. So we start this series to  explore solutions for questions in Leetcode.

Today’s question is [Two Sum](https://leetcode.com/problems/two-sum/). You can get details following the link.

The first solution is very straitforward to follow the equation `nums[0] + nums[1] = 2 + 7 = 9`.

```swift
func twoSum(nums: [Int]?, _ target: Int) -> [Int]? {
        guard let nums = nums, nums.count > 1 else { return nil }

        var res = [Int]()
        let count = nums.count
        for i in 0..<count {
            for j in i+1..<count {
                if (nums[i] + nums[j] == target) {
                    res.append(i)
                    res.append(j)
                }
            }
        return res
    }
```

We can easily get the time complexity is O(n^2) because there are two nested loops which traverse the array.

Let’s change the equation to `nums[1] = target - nums[0]` and make it more general to `nums[j] = target - nums[i]`. So the question is changed to find index `j`whose value is `target - nums[i]`. Here we need a dictionary whose key is the value in the array and value is the corresponding index.

```swift
var map = [Int: Int]()
for i in 0..<count {
     map[nums[i]] = i
 }
```

So there are two tricks. The first one is change the equation to `nums[j] = target - nums[I]`. And the second is define a dictionary which stores the mapping between each value and its indexes. Please remember these two tricks especially the second one which we will also use in coming articles.

The second solution

```swift
func twoSum1(nums: [Int]?, _ target: Int) -> [Int]? {
        guard let nums = nums, nums.count > 1 else { return nil }

        var res = [Int]()
        let count = nums.count

        var map = [Int: Int]()
        for i in 0..<count {
            map[nums[i]] = i
        }

        for i in 0..<count {
            let value = target - nums[i]
            if let index = map[value], index != i {
                res.append(i)
                res.append(index)
            }
        }

        return res
    }
```

Combine two loops to make it more clean

```swift
func twoSum(nums: [Int]?, _ target: Int) -> [Int]? {

        guard let nums = nums, nums.count > 1 else { return nil }

        var map = [Int: Int]()
        for i in 0 ..< nums.count {
            if let value = map[nums[i]] {
                return [value, i]
            }
            map[target - nums[i]] = i
        }
        return nil
    }
```

The time complexity is improved to O(n), but we use a dictionary to store the map between value and its indexes.

This is a simple start. We will deep dive into Leetcode with Swift.
