# Operations (Part 2)
Slides ([link](https://docs.google.com/presentation/u/2/d/1ALEIXU57cVmmrW6K6L5Utzw92P9NOBphU_Y3uADd8rs/copy?id=1ALEIXU57cVmmrW6K6L5Utzw92P9NOBphU_Y3uADd8rs&copyCollaborators=false&copyComments=false&includeResolvedCommentsOnCopy=false&title=Copy%20of%20L07&copyDestination=1fY3GqrOLSrg6bNkuKgAX7vvL-bRFv95a&token=AC4w5Vj8kT50sjwQY3YwuC4Ui99u3Br4zg%3A1560890410248&usp=slides_web))

Github class repo ([link](https://github.com/Make-School-Courses/MOB-2.3-Concurrency-Parallelism-in-iOS/blob/master/Lessons/07-Operations-Pt2/Lesson7.md))

# How to implement Operations
## Synchronous

Operation objects run synchronously by default.
In a synchronous operation:
* The operation object does not create a separate thread on which to run.
* When calling start(), the operation executes `immediately` in the `current thread`.
* By the time the start() method returns control to the caller, the task is complete.

## Asynchronous
An asynchronous operation object:
* Is responsible for scheduling its task on a `separate` thread. The operation could do that by:
	- starting a new thread directly
	- calling an asynchronous method
	- submitting a block to a dispatch queue for execution
* When you call the start() method of an asynchronous operation, that method `may return before the corresponding task is completed`. It does not actually matter if the operation is ongoing when control returns to the caller, only that it could be ongoing.

> Defining an asynchronous operation requires more work because you have to monitor the ongoing state of your task and report changes in that state using KVO notifications.

When you add an operation to an operation queue, the queue ignores the value of the `isAsynchronous` property and always calls the start() method from a separate thread.
* Thus, if you always run operations by adding them to an operation queue, there is no reason to make them asynchronous.

# Are Asynchronous and Concurrent the same thing?
`Concurrency` is when the execution of multiple tasks is interleaved, instead of each task being executed sequentially one after another.

`Parallelism` is when these tasks are actually being executed in parallel.

![p-vs-c](/img/p-vs-c.png)

StackOverflow ([source](https://stackoverflow.com/questions/4844637/what-is-the-difference-between-concurrency-parallelism-and-asynchronous-methods))

# Serial and Concurrent
`About the number of threads available to a queue.`<br>
* `Serial` queues only have a `single thread` associated with them and thus only allow a single task to be executed at any given time.
* `Concurrent` queues can utilize `as many threads as the system has available resources for`. On a concurrent queue, threads will be created and released as needed.

>Is about waiting.
Whether or not the queue on which you run your task has to wait for your task to complete before it executes other tasks.
You can submit asynchronous or synchronous task to either a serial queue or a concurrent queue.

# Example
> First

```Swift
class FilterOperation: Operation {
    let flatigram: Flatigram
    let filter: String

    init(flatigram: Flatigram, filter: String) {
        self.flatigram = flatigram
        self.filter = filter
    }

    override func main() {
        if let filteredImage = self.flatigram.image?.filter(with: filter) {
            self.flatigram.image = filteredImage
        }
    }
}
```

> Second

```Swift
class MyConcurrentOperation: Operation {
override var isAsynchronous: Bool { return true }
override var isExecuting: Bool { return state == .executing }
override var isFinished: Bool { return state == .finished }

/// Everything else here.
override func start() {
  if self.isCancelled {
    state = .finished
  } else {
    state = .ready
    main()
  }
}
override func main() {
  if self.isCancelled {
    state = .finished
  } else {
    state = .executing
  }
}
}
```

# In Class Activity
> Just needed to add 2 print statements

```Swift
import UIKit
import PlaygroundSupport

PlaygroundPage.current.needsIndefiniteExecution = true

// Queue
let operationQueue = OperationQueue()
operationQueue.qualityOfService = .userInitiated

class MyOperation: Operation {

 //TODO: Create main()
 override func main() {
     print("My operation started")
 }
}

let myOp = MyOperation()

myOp.completionBlock = {
 //TODO: print "MyOp Completed"
 print("MyOp Completed")
}

operationQueue.addOperation(myOp)
```

> Output

```
My operation started
MyOp Completed
```

# OperationQueues

* Easiest way to execute operations.
* Powerful: lets us control QoS levels, how many operations can execute simultaneously…
* Manage the scheduling and execution of an Operation.
* Set the max number of operations that can run simultaneously on a queue.

```Swift
class OperationQueue: NSObject
```

> We can execute tasks concurrently, like with GCD and DisptachQueues, but in an object-oriented fashion.

## Different Behavior
1. No serial queues - By default, all OperationQueues operate concurrently.
1. Developer Control
  * maxConcurrentOperationCound
  * Cancel
  * Pause
  * queuePriority
  * QoS
1. Determining Execution Order - do not strictly conform to FIFO, but as a prioritized FIFO queue

## Prioritized FIFO queue
* Operations within an operation queue are organized according to their `readiness`, `priority level`, and `dependencies`, and are executed based on those criteria.
* `You can set priority on individual operations`. Those with the highest priority get pushed ahead, but not necessarily to the front of the queue.
* `Operations with the same priority get executed in the order they were added to the queue` — unless an operation has dependencies, which allow you to define that some operations will only be executed after the completion of the other operations they are dependent on.

## Readiness

> If all the queued operations have the same `queuePriority` and their state is `isReady = true`, they are executed in the order `in which they were submitted to the queue`.<br>
Otherwise, the operation queue always executes the one with the highest priority relative to the other ready operations.

## Lifecycle Notes
> After being added to an operation queue, an operation remains in its queue until it reports that it is finished with its task. <br>
`You can’t directly remove an operation from a queue after it has been added.`<br>
Operation queues retain operations until they're finished, and queues themselves are retained until all operations are finished.


## Thread Safety

> Operation queues use the Dispatch framework to initiate the execution of their operations.
As a result, operations are always executed on a separate thread, regardless of whether they are designated as synchronous or asynchronous.<br>
This means `Operation queues are inherently thread safe`: You can safely access a single OperationQueue object from multiple threads without creating additional locks to synchronize access to it.

# In Class Activity II
> Just need to answer these questions.
* Operations Q & A ([original](https://docs.google.com/document/d/1wpsHJTMJnTL_Qhcb5y3fFAfEfARt3YrtlWRPQB9JNzs/edit?usp=sharing), [My version](https://docs.google.com/document/u/2/d/1CI0EVVArHxcEqbYUh6-j4czafNCfuQ8o9Az678CD8VE/edit))

# Homework
* Solve [Dining Problem](https://github.com/raywenderlich/swift-algorithm-club/tree/master/DiningPhilosophers)
* Finish [Philosophers' Problem](https://github.com/raywenderlich/swift-algorithm-club/tree/master/DiningPhilosophers)
