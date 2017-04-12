---
layout: post
title:  "Swift World: Change your app’s icon programmatically in iOS 10.3"
date:   2017-04-12 20:00:00
categories: tutorial
---

The icon of Apple’s official Clock app is amazing because it gives us a running clock. Although we can’t get this with public API, a small step forward has been taken in iOS 10.3. We can change our app’s icon programmatically.

Let’s get started.

### Prerequisite

Obviously, we need to prepare different icons. There are some requirements. The first one is size. For example, if our app is only for iPhone, the sizes are 60pt@2x and 60pt@3x which means 120*120 and 180*180. And its names should follow the rules like icon1@2x.png and icon1@3x.png. The second one is where we put the alternate icons. Generally, we put our app icon in Assets.xcassets, but not for the alternative icons. We usually put it in a group specifically for alternative icons.

![group](http://pengguo.xyz/resources/ChangeIcon1.png)

The alternative app icon in our example is the following image.

![AlternateIcon](http://pengguo.xyz/resources/AlternateIcon.png)

### Register in Info.plist

As the first step, we need to register our alternative icons in Info.plist. Normally, we put app icon in Assets.xcassets and alternative icons in a specific group. For example the alternate icon in our example is AlternateIcon.png in group AleternateIcon.  The following steps are taken to register it.

1. Add row and select “Icon files (iOS 5)” in Info.plist.
2. Add new entry CFBundleAlternateIcons
3. Add new entry for alternative icon
4. Add file name to item in CFBundleIconFiles

The final Info.plist is like the following.

![InfoPlist](http://pengguo.xyz/resources/InfoPlist.png)

The new dictionary entry CFBundleAlternateIcons is specific for alternative icons. It works with previous keys like CFBundleIcons and CFBundlePrimaryIcon to record the informations about icons. In every categories, the array value for key CFBundleIconFiles includes the icon files.

The Info.plist in raw format will show us the structure clearly.

```xml
<key>CFBundleIcons</key>
	<dict>
		<key>CFBundleAlternateIcons</key>
		<dict>
			<key>AlternateIcon</key>
			<dict>
				<key>CFBundleIconFiles</key>
				<array>
					<string>AlternateIcon</string>
				</array>
			</dict>
		</dict>
		<key>CFBundlePrimaryIcon</key>
		<dict>
			<key>CFBundleIconFiles</key>
			<array/>
			<key>UIPrerenderedIcon</key>
			<false/>
		</dict>
	</dict>
```


[Information Property List Key Reference](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW14) gives detailed explanation. The relevant sections are CFBundleIcons, Contents of the CFBundlePrimaryIcon Dictionary Entry and Contents of the CFBundleAlternateIcons Dictionary Entry.

### work in code

Let’s start coding. The following is new APIs we will use.

`UIApplication.shared.supportsAlternateIcons` is

> A Boolean value indicating whether the app is allowed to change its icon. - from [Apple Developer Documentation](https://developer.apple.com/reference/uikit/uiapplication/2806815-supportsalternateicons)

```
func setAlternateIconName(_ alternateIconName: String?,
        completionHandler: ((Error?) -> Void)? = nil)
```

> Use this method to change your app's icon to its primary icon or to one of its alternate icons. You can change the icon only if the value of the supportsAlternateIcons property is true. - from [Apple Developer Documentation](https://developer.apple.com/reference/uikit/uiapplication/2806818-setalternateiconname)

`UIApplication.shared.setAlternateIconName(nil)` means use primary app icon.

We add a toggle button to change the icon

```swift
@IBOutlet weak var changeIconBtn: UIButton!
...
@IBAction func changeIcon(_ sender: UIButton) {
        if UIApplication.shared.supportsAlternateIcons {
            if let alternateIconName = UIApplication.shared.alternateIconName {
                print("current icon is \(alternateIconName), change to primary icon")
                UIApplication.shared.setAlternateIconName(nil)
            } else {
                print("current icon is primary icon, change to alternative icon")
                UIApplication.shared.setAlternateIconName("AlternateIcon"){ error in
                    if let error = error {
                        print(error.localizedDescription)
                    } else {
                        print("Done!")
                    }
                }
            }
        }
    }
```

The final result is shown in the video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZFjkLoM-vAw" frameborder="0" allowfullscreen></iframe>

Thanks for your time.
