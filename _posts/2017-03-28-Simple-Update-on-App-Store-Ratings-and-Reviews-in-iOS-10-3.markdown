---
layout: post
title:  "Simple Update on App Store Ratings and Reviews in iOS 10.3"
date:   2017-03-28 20:00:00
categories: tutorial
---

iOS 10.3 was released today. It introduced SKStoreReviewController which brought new App Store rating and reviews way to app’s users. Before that, developers use their own or third-party components to take users to App Store . We will introduce this new way with a simple example.

First, we need latest Xcode 8.3 and start a new Single View Application.  
Then in ViewController.swift, import StoreKit and add one line in ViewDidLoad as following code snippet.

```
import StoreKit

...

override func viewDidLoad() {
        super.viewDidLoad()
        SKStoreReviewController.requestReview()
    }
```

Build and run

![StoreReview1](http://pengguo.xyz/resources/StoreReview1.png)

Rate it

![StoreReview2](http://pengguo.xyz/resources/StoreReview2.png)

For further details, please go to [reference page](https://developer.apple.com/reference/storekit/skstorereviewcontroller/2851536-requestreview) for request​Review.
And another advantage for our developers is

> When iOS 10.3 ships to customers, you will be able to respond to customer reviews on the App Store in a way that is available for all customers to see.

[This article](http://daringfireball.net/2017/01/new_app_store_review_features) by John Gruber explained some details on new App Store review.

Thanks for your time.
