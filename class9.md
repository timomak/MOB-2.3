# Pitfalls
Github Class Repo ([link](https://github.com/Make-School-Courses/MOB-2.3-Concurrency-Parallelism-in-iOS/blob/master/Lessons/09-Pitfalls-Challenges/Lesson9.md))

Class Slides ([link](https://docs.google.com/presentation/u/2/d/1JvRyyZy50ndg0__U_dAQkIar78tIHrIJ_qK8FnL28SM/copy?id=1JvRyyZy50ndg0__U_dAQkIar78tIHrIJ_qK8FnL28SM&copyCollaborators=false&copyComments=false&includeResolvedCommentsOnCopy=false&title=Copy%20of%20L09&copyDestination=1fY3GqrOLSrg6bNkuKgAX7vvL-bRFv95a&token=AC4w5Vhos6RqPEXTBH013ywGjIgKbmZP8A%3A1561495643049&usp=slides_web))

> Working with the same variable on two different different threads can be unpredictable. But it can be avoided.

**Solution**

```Swift
private let countQueue = DispatchQueue(label: "countqueue")
private var count = 0
public var count: Int {
  get {
    return countQueue.sync {
      count
    }
  }
  set {
    countQueue.sync {
      count = newValue
    }
  }
}
```

# In class Activity
What is Concurrency?

> Things happening at the same time.
* It allows you to run smaller / easier processes elsewhere from the main thread.
  * UI can be done quickly using concurrency


What is Parallelism?
* Simultaneously running multiple processes.
* Parallelism uses concurrency to operate.
* Concurrency breaks down large tasks into multiple pieces. Parallelism takes individual pieces and processes them simultaneously.


What are most commonly used APIs to implement concurrency in iOS?
* Grand Central Dispatch (GCD) is a low-level API for managing concurrent operations.
* It is an implementation of task parallelism based on the Thread Pool design pattern.



What is a queue? What is their relationship with FIFO?

> In Computer Science, a queue is a data structure that manages a collection of objects in FIFO 3 order, where the first object added to the queue is the first object removed from (executed by) the queue.

![queue-line](img/queue-line.png)

In GCD, `DispatchQueue` is a queue object that manages the execution of tasks on your app's `main` thread or on a `background thread`.

It is a FIFO queue to which your application can submit tasks in the form of block objects (functions or closures).



What are all the different types of queues and their priorities?
What is the difference between an asynchronous and a synchronous task?
What is the difference between a serial and a concurrent queue?
How does GCD work?
Explain the relationship between a process, a thread and a task.
Are there any threads running by default? Which ones?
How does iOS support multithreading?
What is NSOperation? and NSOperationQueue?
What is QoS?
Explain priority inversion.
Explain dependencies in Operations.
When do you use GCD vs Operations?
How do we know if we have a race condition?
What is deadlock?
What is context switching in multithreading?
What are the ways we can execute an Operation? How are they different?
What is DispatchSemaphore and when can we use it?
What happens if you call sync() on the current or main queue?
