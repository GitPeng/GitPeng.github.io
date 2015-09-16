---
layout: post
title:  "Interaction With Apple TV II : Gestures and Button Presses"
date:   2015-09-16 20:00:00
categories: tutorial
---
In [Hello Apple TV!](http://pragma.tv/jekyll/update/2015/09/10/Hello-Apple-TV.html),
we changed the text's color by pressing the play/pause button of Apple TV Remote. In [the previous article](http://pragma.tv/tutorial/2015/09/12/Interaction-With-Apple-TV-I-Focus.html), we simply introduced focus and mentioned the trigger is user's operations like swiping on touchpad.
So pressing buttons and making gestures are two ways user drive the UI on screen.

Behind the gestures and button presses are UIGestureRecognizer which we're familiar with in iOS.
The difference is allowedPressTypes. This is the code snippet we used in [Hello Apple TV!](http://pragma.tv/jekyll/update/2015/09/10/Hello-Apple-TV.html). When we defined a tap gesture,
the allowedPressTypes is assigned to indicate the recognizer is triggered only by play/pause button.

    UITapGestureRecognizer* tapRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(tapped)];
    tapRecognizer.allowedPressTypes = @[[NSNumber numberWithInteger:UIPressTypePlayPause] ];
    [self.view addGestureRecognizer:tapRecognizer];

Swift version:

    let tapRecognizer = UITapGestureRecognizer(target: self, action:"tapped")
    tapRecognizer.allowedPressTypes = [NSNumber(integer: UIPressType.PlayPause.rawValue)]
    self.view.addGestureRecognizer(tapRecognizer)

   ![HelloTV.gif](https://db.tt/GXGS6Y03)

Of course, other buttons has their own press type values.

Then another import gesture is swiping or panning on touchpad. The system use it to trigger focus movement as we described in [the previous article](http://pragma.tv/tutorial/2015/09/12/Interaction-With-Apple-TV-I-Focus.html). The code snippet
is also simple.
    UISwipeGestureRecognizer* swipeRecognizer = [[UISwipeGestureRecognizer alloc] initWithTarget:self action:@selector(swipped)];
    swipeRecognizer.direction = UISwipeGestureRecognizerDirectionRight;
    [self.view addGestureRecognizer:swipeRecognizer];

Swift version:

    let swipeRecognizer = UISwipeGestureRecognizer(target: self, action:"swiped:")
    swipeRecognizer.direction = .Right
    self.view.addGestureRecognizer(swipeRecognizer)

 ![swipe-gesture](https://db.tt/oWGECPL9)

 Do you remember we can customize our own gesture with UITouch in iOS? Accordingly, we can deal with the different button press phases. Let's continue our example in Hello Apple TV to show text in different colors in different phases.

 Please delete the recognizer related code and add code snippets below.

    - (void)pressesBegan:(NSSet<UIPress *> *)presses withEvent:(UIPressesEvent *)event
    {
        for (UIPress *item in presses) {
            if (item.type == UIPressTypePlayPause) {
                self.label.textColor = [UIColor redColor];
            }
        }
    }

    - (void)pressesEnded:(NSSet<UIPress *> *)presses withEvent:(UIPressesEvent *)event
    {
        for (UIPress *item in presses) {
            if (item.type == UIPressTypePlayPause) {
                self.label.textColor = [UIColor greenColor];
            }
        }
     }

    - (void)pressesChanged:(NSSet<UIPress *> *)presses withEvent:(UIPressesEvent *)event
    {
        // ignore
    }

    - (void)pressesCancelled:(NSSet<UIPress *> *)presses withEvent:(UIPressesEvent *)event
    {
        for (UIPress *item in presses) {
            if (item.type == UIPressTypePlayPause) {
                self.label.textColor = [UIColor blackColor];
            }
        }
    }

Swift version

  override func pressesBegan(presses: Set<UIPress>, withEvent event: UIPressesEvent?) {
      for item in presses
      {
          if item.type == UIPressType.Select
          {
              self.label.textColor = UIColor.redColor()
          }
      }
  }

  override func pressesEnded(presses: Set<UIPress>, withEvent event: UIPressesEvent?) {
      for item in presses
      {
          if item.type == UIPressType.Select
          {
              self.label.textColor = UIColor.greenColor()
          }
      }
  }

  override func pressesChanged(presses: Set<UIPress>, withEvent event: UIPressesEvent?) {
      // ignored
  }

  override func pressesCancelled(presses: Set<UIPress>, withEvent event: UIPressesEvent?) {
      for item in presses
      {
          if item.type == UIPressType.Select
          {
              self.view.backgroundColor = UIColor.blackColor()
          }
      }
  }

Build and run.
 ![UIPress](https://db.tt/eXTk8MPK)
