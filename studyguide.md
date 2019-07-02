# Study Guide for MOB 2.3 (Concurrency & Parallelism)

> Use all the notes provided to learn, this is just a summary.

Quizlet ([link](https://quizlet.com/_6u0szm))

# Learning Material

## Definition
* `asynchronous` - In cases where you want to ensure that a manually executed operation does not block the calling thread, you should define your operation as asynchronous
* `Semaphore` - Invented by Edsger Dijkstra, this is a data structure that is useful for solving a variety of synchronization problems.
* `Synchronous` - submits a task for execution on the current queue and returns control to the calling function only after that code block (task) finishes executing. Your app will wait and block the current thread's run loop until execution of the current task finishes, before returning control to the current queue and executing the next task.
* `Amdahl's Law` - This is a formula which gives the theoretical speedup in latency of the execution of a task at fixed workload that can be expected of a system whose resources are improved.
* `Asynchronous` - Schedules a task for immediate execution, and immediately returns control to the calling function. No waiting, no blocking....
* `Serial` - If you do not expressly define your DispatchQueue with the .concurrent attribute, GCD will by default create a Serial queue
* `Thread Pool` - Often also called a "replicated workers" or "worker-crew model," this type of software design pattern for achieving concurrency of execution in a computer program consists of multiple threads waiting for tasks to be allocated for concurrent execution by the supervising program.
* `GCD` - This is Apple's implementation of C's libdispatch library, and it runs directly in the UNIX layer of iOS.
* `BlockOperation` - This construct can be thought of as a bridge between GCD DispatchQueues and Operations because it:
  - it manages the concurrent execution of one or more closures on the default global queue.
  - lets you take advantage of all the other features of an operation: cancelling a task, reporting task state, specifying dependences, etc.
* `Dispatch Queue` - This mechanism hides all thread management related activities, is thread safe, maintains a queue of tasks and executes those tasks, either serially or concurrently, in their turn.
* `A race condition` - When multiple threads are trying to write to the same variable at the exact same time, you could have A race condition
* `unblocked Semaphore` - When a thread increments the semaphore, if there are other threads waiting, one of the waiting threads gets unblocked
* `.background` - The lowest QoS level is .background
* `An Operation` - This construct:
  - describes a single unit of work
  - is a higher level of abstraction over GCD
  - is Object-oriented (vs functions/closures in GCD)
  - Executes concurrently — but can be serial by using dependencies
  - Offers more developer control (than GCD)
* `Thunk` - In computer programming, this is a subroutine used to inject an additional calculation into another subroutine primarily used to delay a calculation until its result is needed, or to insert operations at the beginning or end of the other subroutine.
* `pool` - Instead of creating a new thread whenever a task is to be executed, then destroying it when the task finishes, available threads are taken from a pool of available threads created and managed by the operating system.
* `Thread-safe` - An attribute describing code that only manipulates shared data structures in a manner that ensures that all threads behave properly and fulfill their design specifications without unintended interaction.
* `Deadlock` - A common problem in multiprocessing systems, parallel computing, and distributed systems, a Deadlock is a state in which each member of a group is waiting for another member, including itself, to take action, such as sending a message or more commonly releasing a lock
* `underlyingQueue` - Before you add operations to an OperationQueue, you can specify an existing DispatchQueue as The dispatch queue used to execute operations by setting the underlyingQueue property.
* `NSInvocationOperation` - The Operation class has 2 pre-defined subclasses: BlockOperation and NSInvocationOperation
* `An Operation Queue` - The easiest way to execute operations is to use An Operation Queue
* `The Critical Section` - In concurrent programming, multiple concurrent accesses to shared resources can lead to unexpected or erroneous behavior. So parts of the program where the shared resource is accessed are protected from concurrent access.
* `GCD` - This is a lightweight way to represent units of work that are going to be executed concurrently, and it uses closures to handle what runs on another thread.
* `Tasks` - In GCD, these can be expressed either as a function or as an anonymous "block" of code (eg, a closure).
* `Thread-safe` - Operations are always executed on a separate thread, regardless of whether they are designated as synchronous or asynchronous. This means Operation queues are inherently Thread-safe
* `Dependencies` - A key advantage of Operations over GCD is that an Operation can have Dependencies which enables developers to execute tasks in a specific order.
* `Dispatch Group` - To resolve race conditions where UI tasks are dependent on slow- or long-running data-related tasks, you could use a Dispatch Group
* `synchronously` - Unlike GCD, operations run synchronously by default.
* `A Process` - The runtime instance of an application with its own virtual memory space and system resources that are independent of those assigned to other programs.
* `DispatchWorkItem` - This built-in class from Apple encapsulates work to be performed on a dispatch queue or within a dispatch group. You can also use an instance of this class as a DispatchSource event, registration, or cancellation handler.
* `NSLock` - The NSLock object implements a basic mutex for Cocoa applications.
* `The Operation Class` - An abstract class that allows you to encapsulate (wrap) a unit of work into a package that you can submit for execution at some time in the future.
* `Default` - The priority level of this QoS falls between .user-initiated and .utility. The GCD global queue runs at this level, as does any work that has no QoS information assigned to it. This QoS is not intended to be used by developers to classify work.
* `synchronously or asynchronously` - Tasks placed into a queue can either run synchronously or asynchronously
* `dispatch queues` - Rather than working with threads directly, GCD offers you an efficient mechanism for executing code concurrently on multicore hardware by submitting work to dispatch queues managed by the system
* `Operation` - Rather than pass closures or functions, this mechanism allows you to pass objects that encapsulate data and functionality to a queue.
* `blocks itself` - If the result is negative when a thread decrements the semaphore, the thread blocks itself and cannot continue until another thread increments the semaphore.
* `dispatch_after` - This function waits until the specified time and then asynchronously adds a block to the specified queue.
* `dispatchMain()` - This function "parks" the main thread and waits for blocks to be submitted to the main queue.
* `asynchronous` - The asynchronous operation object that is responsible for scheduling its task on a separate thread.
* `GCD` - Good to use for simple, common tasks that need to be run only once and in the background.
* `The Async/Await Pattern` - This is a syntactic feature of many programming languages that allows an asynchronous, non-blocking function to be structured in a way similar to an ordinary synchronous function (and it may be included in future versions of Swift).
* `Priority Inversion` - This condition most commonly occurs in iOS when a queue with a lower quality of service is given a higher system priority than a queue with a higher QoS.
* `Dispatch` - is another name Apple uses for Grand Central Dispatch (GCD).
* `mutex` - A mutex is like a token that passes from one thread to another, allowing one thread at a time to proceed.
* `Dispatch Group` - A Dispatch Group is a group of tasks that you monitor as a single unit. It allows you to group together multiple tasks and either wait for them to complete or to receive a notification once they complete.
* `asynchronous` - If you execute operations manually, you will likely want to define your operation objects as asynchronous
* `Context switch` - The process of storing the state of a process or of a thread so that it can be restored and its execution resumed from the same point later. This allows multiple processes to share a single CPU, and is an essential feature of a multitasking operating system.
* `Concurrency` - The ability to decompose a program, algorithm, or problem into smaller components or units that can be executed out-of-order, or in partial order, without affecting the final outcome.
* `Dispatch Semaphore` - controls access to a resource across multiple execution contexts through use of a traditional counting semaphore.
* `Dispatch Queue` - Work submitted to this mechanism executes on the pool of threads managed by the system.
* `NSBlockOperation` - This operation itself can contain more than one block and the operation will be considered finished when all blocks have completed execution.
* `threadsafe` - Tasks which modify the same resource must not run at the same time, unless the resource is threadsafe
* `.userInteractive` - The highest QoS level
* `Finished` - An Operation can be cancelled in each of its lifecycle states except the Finished state.
* `Task Parallelism` - Concurrency is also known as Task Parallelism
* `concurrently` - By default, all OperationQueues operate concurrently
* `BlockOperation` - object can be used to execute several blocks at once without having to create separate operation objects for each. In this way, it can behave like a GCD DispatchGroup.
* `Task Parallelism` - The characteristic of a parallel program that entirely different calculations can be performed on either the same or different sets of data.
* `Operation(s)` - Allows for greater control over the submitted tasks, including some control over scheduling through adding dependencies among various operations and can re-use, cancel or suspend them.
* `` - This utilizes a shift from procedural tasks, which run sequentially, to tasks that run at the same time.
* `` -
* `` -
* `` -
* `` -
* `` -
* `` -
* `` -
* `` -
* `` -
* `` -
* `` -
* `` -

## Q & A
**Q**: What are two ways to execute operations?<br>
**A**: (1) Operation Queues (2) The start() Method<br><br>

**Q**: What are the only 2 lifecycle states of the Operation class which you can directly manipulate?<br>
**A**: (1) isExecuting (2) isCancelled<br><br>

**Q**: When subclassing the Operation class, what are you required to do with its main() method?<br>
**A**: override it<br><br>

**Q**: If queues execute in FIFO order, in what order do Stacks execute?<br>
**A**: LIFO<br><br>

**Q**: What is the maximum number of cores currently on iOS devices?<br>
**A**: 12<br><br>

**Q**: When subclassing the Operation class, what does the default implementation of the main() method do?<br>
**A**: nothing<br><br>

**Q**: Swift and iOS provide two APIs you can use to improve your app's performance through Concurrency:<br>
**A**: (1) GCD (2) Operations<br><br>

**Q**: Dispatch queues process tasks in what order?<br>
**A**: FIFO<br><br>

**Q**: Which of these is correct about dispatch group tasks:
  a) tasks do not all have to run at the same time.
  b) tasks can even run on different queues.
  c) tasks can be asynchronous or synchronous.
  d) All of the above are correct.
  e) None of the above are correct.<br>
**A**: d) <br><br>

**Q**: Operation Queues allows you to add work to a queue in 3 separate ways.
What function do all 3 ways use?<br>
**A**: The addOperation(_:) function<br><br>

**Q**: If you call sync() on the current or main queue, what happens?<br>
**A**: It could lock up your UI or cause an app crash<br><br>

**Q**: If an iOS device has 8 cores, how many threads can be executing at once on that device?<br>
**A**: 8<br><br>

**Q**: If you always plan to use queues to execute your Operations, is it simpler to define them as synchronous or asynchronous?<br>
**A**: synchronous<br><br>

**Q**: Deadlocks, Race Conditions, and Thread Explosions
are all types of concurrency challenges. Name 2 more...<br>
**A**: Readers-Writers Problem
Priority Inversion<br><br>

**Q**: If you are creating a concurrent operation, what methods and properties must you override (at minimum)?<br>
**A**:
* start()
* isAsynchronous
* isExecuting
* isFinished<br><br>

**Q**: What are the 4 key benefits the Operation class offers over GCD?<br>
**A**: Reusability, Dependencies, KVO-Compliance, and Developer Control<br><br>

**Q**: <br>
**A**: <br><br>

**Q**: <br>
**A**: <br><br>

**Q**: <br>
**A**: <br><br>

**Q**: <br>
**A**: <br><br>

**Q**: <br>
**A**: <br><br>

**Q**: <br>
**A**: <br><br>

**Q**: <br>
**A**: <br><br>

**Q**: <br>
**A**: <br><br>

**Q**: <br>
**A**: <br><br>

**Q**: <br>
**A**: <br><br>

**Q**: <br>
**A**: <br><br>
