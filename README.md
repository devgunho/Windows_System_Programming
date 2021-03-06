# Synchronization

### Thread & Synchronization

**Windows system elements required for thread synchronization**

**Concepts and Needs of Synchronization**

---

- **Elements of Synchronization**

Scheduling processes, Kernel objects, Threads, Thread scheduling
Thread Programming Pattern

>1. CreateThread
>2. ThreadExcept
>3. MyThread

<br/>

- **thread synchronization**

The Meaning and Necessity of Synchronicization from the Perspective of **Atomicity** and **Instruction Reordering**

>4. InterLock
>5. MemBarrier

<br/>

<br/>

### Synchronize with Kernel Objects

**Describes the sync wait functions and sync-only kernel objects provided by Windows**

**Usefulness and utilization of threads or window messages**

-----

- **Thread Synchronization API**

Description of the standby function that waits for threads to synchronize

Understand the relationship between the standby function and the kernel object

> 6.WaitThread
>
> 7.MyThread_ModifyWait
>
> 8.WaitMultiThreads

<br/>

- **Synchronization objects for data protection**

Describe Mutex and Semaphore from the perspective of protection of shared resources

> 9.MutexTest
>
> 10.MutexNoti
>
> 11.SemaphoreTest
>
> 12.TPSemaphore
>
> 13.WQSemaphore

<br/>

- **Synchronization object for flow control**

Description of synchronization-only objects for the purpose of controlling and notifying flows between threads.

> 14.ExitWithEvent
>
> 15.MyThread(3)_ModifyExit
>
> 16.EventTest
>
> 17.MultiSyncWaits
>
> 18.EventNotify