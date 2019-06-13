# Operations (Part 1)
Github Lesson ([link](https://github.com/Make-School-Courses/MOB-2.3-Concurrency-Parallelism-in-iOS/blob/master/Lessons/06-Operations-Pt1/Lesson6.md))

## Homework Review
1) Download and run the [iOS-JankyTable_starter](https://github.com/Make-School-Labs/iOS-JankyTable_starter) app

2) While running, examine the 2 points in the UI flow in which performance currently is most noticeably horrible:
- on initial table view presentation
- whenever the user scrolls the table view

3) Set a breakpoint somewhere in the `cellForRowAt(_:)` function (at ` return cell` on line 56 is a good place):

- when Xcode stops on your breakpoint, examine the Threads and Queues listed in the Debug Navigator in the Navigation Pane. (i.e., find the button for toggling between "View Process by Thread" and "View Process by Queue" and practice viewing each in turn.)

For next class, be prepared to answer these questions:

**Q:** How many threads are active at the breakpoint?

**A:** 7

**Q:** How many queues?

**A:** 1

**Q:** What is the thread number of the `main queue`?

**Q:** What is the *last function* executed prior to the breakpoint?

**Q:** What is the *first function* executed on the `main queue`/`main thread`?

**Q:** Why do you think it takes so long to present the images to the user?

**Q:** Why is scrolling so slow?

**A:** Each time you load a cell, it downloads the picture again.

**Q:** What could you do to resolve these 2 egregious UI issues?

# Operation Class
> **Operation** is an abstract class that allows you to **encapsulate** a **unit of work** into a **package** that you can submit for execution at some time in the **future**.

```Swift
class Operation : NSObject
```

## Key Attributes
* An Operation describes a single unit of work
* A higher level of abstraction over GCD
* Object-oriented (vs functions/closures in GCD)
* Execute concurrently — but can be serial by using dependencies
* Offer more developer control (than GCD)

## Reusability
> Instances of concrete Operation subclasses are "once and done" tasks. This means that once an `Operation` object is added to an `OperationQueue`, the same object cannot be added to any other `OperationQueue`; the specific task represented by that particular object cannot be executed twice.

> But, because an instance of `Operation` is an actual Swift object representing a unit of work, you can easily submit that unit of work multiple times by creating and sending new objects of that same Operation subclass, if needed.

# In Class Activity 1
> After Running the code, answer the questions.

## Solution:
```Swift
import Foundation

let printerOperation = BlockOperation() // 1) create printerOperation as BlockOperation

// 2) add code blocks to the operation
printerOperation.addExecutionBlock { print("I") }
printerOperation.addExecutionBlock { print("am") }
printerOperation.addExecutionBlock { print("printing") }
printerOperation.addExecutionBlock { print("block") }
printerOperation.addExecutionBlock { print("operation") }

printerOperation.completionBlock = { // 3) set completion block
    print("I'm done printing")
}

let operationQueue = OperationQueue() // 4) Create an OperationQueue
operationQueue.addOperation(printerOperation) // 5) add operation to queue

/**

 TODO: Run the code as a playground a few times and observe results...

 Q: What did you notice about the order in which the submitted blocks execute?
 A: Not in order
 Q: How about the completionBlock's execution order?
 A: It's at the end
 **/
```


# In Class Activity 2
Solution:

```Swift
import Foundation

let phrase = "Mobile is the greatest!"
let tokenOperation = BlockOperation()

for token in phrase.split(separator: " ") {
    tokenOperation.addExecutionBlock {
        print(token)
        sleep(2)
    }
}

tokenOperation.completionBlock = {
    print("All operations completed!")
}

duration {
    tokenOperation.start()
}
```

# After Class
1. Research:
- [Concurrent Versus Non-concurrent Operations - Apple docs](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW1)
- `NSInvocationOperation` object
- Passing Data Between Operations
- [KVO-Compliant Properties: (of the `Operation` class) - Apple docs](https://developer.apple.com/documentation/foundation/operation)
- [completionBlock - Apple docs](https://developer.apple.com/documentation/foundation/operation/1408085-completionblock)
- [Tips for Implementing Operation Objects - Apple docs](https://developer.apple.com/library/archive/documentation/General/Conceptual/ConcurrencyProgrammingGuide/OperationObjects/OperationObjects.html#//apple_ref/doc/uid/TP40008091-CH101-SW35)

2. Assignment:
- Resume your solution to the issues with **JankyTable app** from Lesson 4.
- Resume/start the *Assignment 2: Solve the Dining Philosophers Problem (challenge)* from previous class:
https://github.com/raywenderlich/swift-algorithm-club/tree/master/DiningPhilosophers


## Additional Resources

1. [Slides](https://docs.google.com/presentation/d/1Pyiey3qo-y8ZUUgv4Peh_xFT0FBQcWGKRLg6cjNrSSI/edit?usp=sharing)
2. [Operation - Apple docs](https://developer.apple.com/documentation/foundation/operation)
3. [OperationQueue - Apple docs](https://developer.apple.com/documentation/foundation/operationqueue)
4. [Queue Priority - Apple docs](https://developer.apple.com/documentation/foundation/operation/1411204-queuepriority)
5. [4 Ways To Pass Data Between Operations With Swift - an article](https://marcosantadev.com/4-ways-pass-data-operations-swift/)
6. [BlockOperation - Apple docs](https://developer.apple.com/documentation/foundation/blockoperation)
7. [Blocks Programming Topics - Apple docs](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Blocks/Articles/00_Introduction.html#//apple_ref/doc/uid/TP40007502)
