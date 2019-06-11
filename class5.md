# Semaphores
Github class repo ([link](https://github.com/Make-School-Courses/MOB-2.3-Concurrency-Parallelism-in-iOS/blob/master/Lessons/05-Semaphores/Readme.md))

# Real Life Semaphores

> A system of signals used to communicate visually

- Flags
- Lights (street lights)

> Has to have distinct different signals for when an action is starting and for when it's stopping.

For example the green light to drive and red light to stop.

# A formal definition

After creating a Semaphore, we initialize an integer value.

It can be **incremented** and **decremented**.

A thread decrements the Semaphore.

If it's negative it stops until it becomes positive against


* `Positive`
  * Represents the number or waiting threads or number of decrements before its blocked
* `Negative`
  * Blocked and waiting
* `Zero`
  * No treads waiting, it will stop if it decrements.

> Before decrementing a Semaphores, you can't check if it's going to block.

> You can't check the number of waiting threads.

Why we use them:
* Helps avoid errors
*

# How it works

> To create

```Swift
sem = Semaphore(1)
```

> Only has two functions

```Swift
sem.signal() // increment
sem.wait() // decrement
```

## Example

| **Thread A**    | **Thread B**  |
| -----------     | ------------- |
| statement a1    | sem.wait()    |
| sem.signal()    | statement b1  |

Thread A:
* Starts a task in step 1
* Signals to increase the Semaphores value by one

Thread B:
* Signals the Semaphores to decrement
* Finished it's task in step 2

# Rendezvous Dilemma

> Two people have a date in a park they have never been to before. Arriving separately in the park, they are both surprised to discover that it is a huge area and consequently they cannot find one another. In this situation each person has to choose between waiting in a fixed place in the hope that the other will find them, or else starting to look for the other in the hope that they have chosen to wait somewhere.

## Class Activity Rendezvous

| **Thread A**    | **Thread B**  |
| -----------     | ------------- |
| statement a1    | statement b1  |
| statement a2    | statement b2  |

We want a1 to happen before b2 and b1 before a2.

This is what it's gonna look like in code:

> First Guess:

```Swift
semaphore1 = Semaphore(0)
semaphore2 = Semaphore(0)

// a1 Download Pic
semaphore1.signal() // semaphore1 == 1
semaphore2.wait() // semaphore2 == -1

// b1 Show pic
semaphore2.signal() // semaphore2 == 0
semaphore1.wait() // semaphore1 == 0

// a2 Process Pic
semaphore1.signal() // semaphore1 == 1
semaphore2.wait() // semaphore2 == -1

// b2 Show filtered pic
semaphore2.signal() // semaphore2 == 0
semaphore1.wait() // semaphore1 == 0
```

## Solution

| **Thread A**       | **Thread B**      |
| -----------        | -------------     |
| statement a1       | statement b1      |  
| aArrived.signal()  | bArrived.signal() |
| bArrived.wait()    | aArrived.wait()   |
| statement a2       | statement b2      |

What about this?

| **Thread A**       | **Thread B**      |
| -----------        | -------------     |
| statement a1       | statement b1      |  
| bArrived.wait()    | bArrived.signal() |
| aArrived.signal()  | aArrived.wait()   |
| statement a2       | statement b2      |

Less efficient, since it might have to switch between A and B one time more than necessary. If A arrives first, it waits for B. When B arrives, it wakes A and might proceed immediately to its wait in which case it blocks, allowing A to reach its signal, after which both threads can proceed.

## Deadlock

| **Thread A**       | **Thread B**      |
| -----------        | -------------     |
| statement a1       | statement b1      |  
| bArrived.wait()    | aArrived.wait()   |
| aArrived.signal()  | bArrived.signal() |
| statement a2       | statement b2      |

If A arrives first, it will block at its wait. When B arrives, it will also block, since A wasn’t able to signal aArrived. At this point, neither thread can proceed, and never will. This situation is called a deadlock.

# Mutual Exclusion
The Semaphores can be only accessed by one thread at a time.

It uses a **Mutex**. A **token** that is passed from one thread to another.

Two Mutex functions:
* **Get**
* **Releases**

## Class activity
Enforce mutual exclusion to the shared variable

| **Thread A**           | **Thread B**      |
| -----------            | -------------     |
| count = count + 1      | count = count + 1 |  


## Solution

| **Thread A**       | **Thread B**      |
| -----------        | -------------     |
| mutex.wait()       | mutex.wait()      |  
| count = count + 1  | count = count + 1 |
| mutex.signal()     | mutex.signal()    |

Since mutex is initially 1, whichever thread gets to the wait first will be able to proceed immediately. Of course, the act of waiting on the semaphore has the effect of decrementing it, so the second thread to arrive will have to wait until the first signals.

> Call wait() each time before using the shared resource. We are basically asking the semaphore if the shared resource is available or not. If not, we will wait.

> Call signal() each time after using the shared resource. We are basically signaling the semaphore that we are done interacting with the shared resource.

# DispatchSemaphore

A DispatchSemaphore has two components:
* A threads queue - used by the semaphore to keep track on waiting threads in FIFO order (The first thread entered to the queue will be the first to get access to the shared resource once it is available).
* A counter value - used by the semaphore to decide if a thread should get access to a shared resource or not. The counter value changes when we call signal() or wait() functions.

```Swift
let semaphore = DispatchSemaphore(value: 1)
DispatchQueue.global().async {
    print("Person 1 - wait")
    semaphore.wait()
    print("Person 1 - wait finished")
    sleep(1) // Person 1 playing with Switch
    print("Person 1 - done with Switch")
    semaphore.signal()
}
DispatchQueue.global().async {
    print("Person 2 - wait")
    semaphore.wait()
    print("Person 2 - wait finished")
    sleep(1) // Person 2 playing with Switch
    print("Person 2 - done with Switch")
    semaphore.signal()
}
```
Console Output:

```
Person 2 - wait
Person 1 - wait
Person 1 - wait finished
Person 1 - done with Switch
Person 2 - wait finished
Person 2 - done with Switch
```

## In Class Activity

```Swift
func downloadMovies(numberOfMovies: Int) {

    // Create a semaphore

    // Launch 8 tasks
    // Each task should wait (pretend downloading takes 2 seconds) and inform the console once it's done.
    // Run the tasks on a background thread.
    // Let the semaphore know when you release the resource

}

downloadMovies(numberOfMovies:2)
```



# After Class
1. Research:
  - Meaning of symmetric vs asymmetric solutions.
  - What is meant by "critical section" in a program.
  - Difference between Dispatch Groups and Semaphores?
  - Multiplex (challenge)

2. Assignment: Solve the **Dining Philosophers Problem** (challenge):
  - https://github.com/raywenderlich/swift-algorithm-club/tree/master/DiningPhilosophers


## Wrap Up (5 min)

- Complete reading
- Complete challenges

## Additional Resources

1. [Slides]()
2. "The Little Book of Semaphores" by Allen B. Downey
3. [Dispatch Semaphore from Apple Docs](https://developer.apple.com/documentation/dispatch/dispatchsemaphore)
4. [An article on semaphores](https://medium.com/swiftly-swift/a-quick-look-at-semaphores-6b7b85233ddb)
5. [Semaphore flowchart](https://medium.com/@roykronenfeld/semaphores-in-swift-e296ea80f860)
