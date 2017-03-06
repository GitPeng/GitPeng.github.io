---
layout: post
title:  "Swifty Leetcode: No.167 Two Sum II"
date:   2017-03-06 20:00:00
categories: tutorial
---

We have explained Two Sum in [Swifty Leetcode: No. 1 Two Sum](http://pengguo.xyz/tutorial/2017/03/03/Swifty-Leetcode-No-1-Two-Sum.html). Today, we will solve a similar question Two Sum II. You can find detailed description following this [link](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/). The difference between question I and I I is the array in II is “already sorted in ascending order”. This is advantage we can take. In the solution, anther popular method will be explained.

In the codes, we define two ‘pointers’ first, the start and the end. The start points to the first element in the array and the end points to the last one. Then in a while loop, if the sum of start and end equals to target, we find the result. If the sum is less than target, the start index will forward one step. On the contrary, the end index will take a step in the opposite direction.

The following figure will make it easier to understand.

![Two Sum II](http://pengguo.xyz/resources/TwoSumII.png)

The method is to definite two or more pointers to point different elements in a collection and move them according to different strategies. We will also use this method in future.

```swift
class Solution {
    static func twoSumII(nums: [Int]?, target: Int) -> [Int]? {
        guard let nums = nums, nums.count > 1 else { return nil }
        var start = 0
        var end = nums.count - 1

        while start < end {
            if nums[start] + nums[end] == target {
                return [start + 1, end + 1]
            }

            if nums[start] + nums[end] < target {
                start += 1
            } else {
                end -= 1
            }
        }
        return nil
    }
}
```

Thanks for your time.
