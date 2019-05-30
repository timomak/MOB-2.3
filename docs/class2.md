# Grand Central Dispatch (Part 1)

## How is it possible to have parallelism without concurrency?
> Concurrency - Handling many things at once (structure) [Can be done on software]
> Parallelism - How you handle many things. (execution) [Has to be concurrent, but it's more on a hardware level (such as the number of cores)]

* `Bit-level Parallelism` - Can be thought of as hardware-based parallelism.
* `Data Parallelism` — is parallelism inherent in program loops, which focuses on distributing the data across different computing nodes to be processed in parallel.
* `A Single instruction, multiple data` (SIMD) - is a class of parallel computers... with multiple processing elements that perform the same operation on multiple data points simultaneously.
* `Task Parallelism` — is the characteristic of a parallel program that entirely different calculations can be performed on either the same or different sets of data.
 * This contrasts with Data Parallelism, where the same calculation is performed on the same or different sets of data.

> Note that Concurrency is a type of Task Parallelism where tasks are divided (decomposed) into smaller bits for parallel processing.

### SIMD
![SIMD](img/SIMD.png)

# GCD and Operations

|  Grand Central Dispatch | Operations |
| ------------- | ------------- |
| A *lightweight* way to represent units of work that are going to be executed concurrently. GCD uses **closures** to handle what runs on another thread. |  Operations are **objects** that encapsulate data and functionality. Operations add a little *extra development overhead* compared to GCD. |
| The system takes care of scheduling for you. Adding dependencies, cancelling or suspending code blocks is labor intensive | Allow for greater control over the submitted tasks, including some control over scheduling through adding dependencies among various operations and can re-use, cancel or suspend them.  |
| GCD is good to use for simple, common tasks that need to be run only once and in the background. |  Operations make it easier to do complex tasks. Use them when you need (a) task reusability, (b) considerable communication between tasks, or (c) to closely monitor task execution. |

### And because Operations are build on top of GCD, it is important to master the lower-level API first...

> Note: Where Swift uses closures (functions) to handle the code that runs on another thread, C#, Typescript, Python, JavaScript and other languages use the the more common Async/Await pattern. The original plans for Swift 5.0 included adding the Async/Await pattern, but this was removed from the Swift specification until some future release.

## Why use GCD?
GCD's design improves simplicity, portability and performance.

* It can help you `improve your app’s responsiveness` by deferring computationally expensive tasks from the foreground (main thread) to the background (non-UI threads).
* It `simplifies` the creation and the execution of asynchronous or synchronous tasks.
* It’s a concurrency model that is `*much easier to work with*` than locks and threads.

> Asynchronous <br>
>![Asynchronous](img/asynchronous.png)

> Synchronous
![Synchronous](img/synchronous.png)
