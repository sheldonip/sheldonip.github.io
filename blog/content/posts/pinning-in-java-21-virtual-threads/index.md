---
title: "Pinning in Java 21 Virtual Threads"
date: 2025-02-27T00:15:00Z
draft: false
image: cover.jpg
---

## Introduction

Virtual threads are introduced in Java 21. The Java architect believes the reactive programming would be replaced by virtual threads at last. When we used synchronized and native code in virtual threads, there is a chance of having deadlocks and the application would become unresponsive. We will talk about this event and how to avoid it from happening.

## What happened?

We used `synchronized` code in virtual threads for a blocking task like this:

```java
synchronized(lockObject) {
    frequentIO();
}
```

However, the thread scheduler would block a OS thread. This is called as pinning. If the task is long enough, there is a chance that all the OS threads are waiting for the lock released, but the virtual thread notified by the releasing lock has no resource to acquire the lock.

Similarly it happens when using native code, all threads are waiting but the next waiter has no way to run. Both cases lead to a deadlock scenario and the application becomes unresponsive. 

You can detected by using JDK Flight Recorder (JFR), which emits `jdk.VirtualThreadPinned` if the threads holds longer than 20ms by default.

## Implementation

The [adoption guide](https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html#GUID-04C03FFC-066D-4857-85B9-E5A27A875AF9) addressed a limitation and suggested an alternative for executing a long-lived and frequent code block, by using an explicit lock:

```java
lock.lock();
try {
    frequentIO();
} finally {
    lock.unlock();
}
```

This limitation would be resolved in Java 24+ by [JEP 491](https://openjdk.org/jeps/491).

## Wrapping up

We have addressed the limitation of virtual threads in Java 21. We also showed another way to use virtual threads for time-consuming code blocks. It is worth performing a load test across your service to pinpoint bottlenecks before they lead to downtime. As virtual threads are gaining popularity, we should not ignoring the updates of it.

## References

- https://netflixtechblog.com/java-21-virtual-threads-dude-wheres-my-lock-3052540e231d
- https://docs.oracle.com/en/java/javase/21/core/virtual-threads.html#GUID-04C03FFC-066D-4857-85B9-E5A27A875AF9
- https://openjdk.org/jeps/444
- https://openjdk.org/jeps/491
- https://stackoverflow.com/questions/78318131/do-java-21-virtual-threads-address-the-main-reason-to-switch-to-reactive-single


## Credits

Cover photo by [amirali mirhashemian](https://unsplash.com/@amir_v_ali?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/red-and-yellow-thread-in-needle-kiH-RBm08NQ?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) 
