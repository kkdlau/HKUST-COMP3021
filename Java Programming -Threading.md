# Java Programming - Multi-threading

[TOC]

## Creating Tasks and Threads

* Using lambda expression to implement `Runnable`
* Creating another class and implement `Runnable`


## Static Method: `yield()`
```java
public void run() {
  for(int i=1; i <= lastNum; i++) {
    System.out.print(" " + i);
    Thread.yield();
  }
}
```
* Pause the current thread and let other thread runs first

## Static Method: `sleep()`

* sleep the thread by given time
* remmeber to handle `InterruptedException`

## Instance Method: `join()`

* force one thread to wait for another thread to finish
```java
Thread task = new Thread(...);
task.join();
```


## Thread States

* `NEW`: haven't put into thread pool (i.e. just created and haven't started)
* `RUNNABLE`: executing in JVM
* `BLOCKED`: waiting for `lock`
* `WAITING`: indefinitely waiting for another thread to perform
* `TIMED_WAITING`: same as `WAITING`, but timed
* `TERMINATED`: thread ends 


## Thread Pool

* `Executor`: for executing teask in a thread pool
  * `execute(Runnable)`: execute a runnable and add it into the pool
* `ExecutorService`: a helper interface for handling thread management
  * `shutdown()`: wait for tasks to finish and shuts down the executor
  * `List<Runnable> shutdownNow()`: same as `shutdown()`, but return list of runnable that cannot terminate instantly
  * `isShutdown()`: true if `Executor` has been shut down
  
### `ExecutorService`

* It is a subinterface of `Executor`
* Which provides control and management of`Executor`
* To create an `ExecutorService`, use:
  * `Executors.newCachedThreadPool()`
  * `Executors.newFixedThreadPool(int numberOfThreads)`
* `shutdown()`: it will terminate the executor
  * if this method doesn't be called, the program won't termiate!
* to ensure executor fully terminates all threads, use `isTerminated()` 
  ```java
  executor.shutdown();
  while (!executor.isTerminated()) { }
  // code executes when all threads are terminated
  ```

## synchronized

### synchronized methods

* ensure the method is accessed by a method at the same time

* static method: acquire a lock to lock the class
* instance method: acquire a lock to lock the object instance

```java
public synchronized void deposit(double amount) {
  //
}
```

```java
public void deposit(double amount) {
  synchronized(this) {
    // same
  }
}
```

## `Lock` and `ReentrantLock`

* `Lock`: interface, provides a basic structure of resource lock
* `ReentrantLock(boolean fair)`: Lock class with fairness policy


### locking and unlocking

* `lock()`: acquires the lock
* `unlock()`: releases the lock

:::info
For good practising, using `try...catch` can make sure the lock is released even if there is any error occurs.
:::

:::info
**Why we need to handle interruption error? When will it occur?**

Usually when a thread is on WAITING, and the thread is shut down for some reason (e.g. executor is shut down), then the thread will be focused to quit and interruption error happens!
:::


### Condition

* Using lock instance to create a new `Condition`
* Condition **must come up with a lock**
* `await()`: pause the thread until receiving the signal
  * after waking up, it will acquire the lock atomically and automatically
