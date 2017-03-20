---
layout: post
title:  "Swift World: Design Patterns - Command"
date:   2017-03-20 20:00:00
categories: tutorial
---

From today, we will talk about behavioral patterns which focus on the communication between objects. It includes command, mediator, observer, state, strategy, etc.

In this article, we will introduce command pattern. As in real world, a command is  a request sent from its invoker (aka caller)  to its receiver. By isolating and encapsulating the request, this pattern separates the invoker and the receiver as the following figure depicts.

> ![CommandPattern](https://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Command_pattern.svg/1400px-Command_pattern.svg.png)
> from [Command pattern - Wikipedia](https://en.wikipedia.org/wiki/Command_pattern)

Then what advantage does it give us?  Let’s get started with our stories on the car.

```swift
protocol Car {
    func move()
}

class Sedan: Car {
    func move() {
        print("Sedan is moving.")
    }
}

class SUV: Car {
    func move() {
        print("SUV is moving.")
    }
}
```

Now,  you, as an iOS app developer,  have a sedan and drive it daily.

```swift
class Developer {
    let sedan = Sedan()
    func driveCar() {
        sedan.move()
    }
}
```

One day, you make so much money from App Store that you buy another SUV.  If you want to drive it, you need to add an instance in the class.

```swift
class Developer {
    let sedan = Sedan()
    let suv = SUV()

    func driveSedan() {
        sedan.move()
    }

    func driveSUV() {
        suv.move()
    }
}
```

But what if your app becomes popular to make you a millionaire one day? You have so many nice cars that your class becomes bloated. It’s command pattern’s showtime.

```swift
protocol Command {
    func execute()
}

class MovementCommand: Command {
    var car: Car

    init(car: Car) {
        self.car = car
    }

    func execute() {
        car.move()
    }
}
```

In the command, we put in the car which is the receiver and the request to get the car to move. Then all you need to do is to replace all the cars with a command instance.

```swift
class Developer {
    var command: Command

    init(command: Command) {
        self.command = command
    }

    func drive() {
        command.execute()
    }
}
```

Let's  see how to use it.

```swift
let sedan = Sedan()
let sedanMovementCommand = MovementCommand(car: sedan)

let suv = SUV()
let suvMovementCommand = MovementCommand(car: suv)

let developer = Developer(command: sedanMovementCommand)
developer.drive()
```

The command pattern is so popular in iOS app development that there is a specific class NSInvocation to help create command. You can refer to its  [document](https://developer.apple.com/reference/foundation/nsinvocation) for detailed explanation. Even it is not available in Swift, we can take it as example of command pattern.

Thanks for your time.
