# Kernel locking vs atomic

Sources:
- https://dit.unitn.it/~abeni/RTOS/2014/locking.pdf?utm_source=chatgpt.com
- chatgpt

## Intro
Before kernel version 2.6 the kernel would not switch task until current task
was completed. Mutual exclusion was not yet a problem. With the rise of multicore
and multithread CPU's that changed. Multiple tasks can execute inside the kernel
simultaneously.

## Atomic context
Atomic context means executing a part of the code that must run without interruption or
delay to maintain correctness and avoid rac conditions. In other words it must not sleep
or block.

## Blocking Mutexes
A task trying to acquire a locked mutex is blocked, which means it gets removed from
the runqueue, until some event wakes it. But there a contexts which can't be blocked,
because the for example hold resources that the blocking task needs to complete.
An example for that would be blocking when interrupts are disabled, that could prevent
the interrupt that would wake the task from running. Some tasks also need efficient and
fast (for example IRQ handlers). Using blocking adds costs like scheduler bookkeeping or
cache effects.

## Spinning Locks
When calling lock on an already locked spinlock the current task spinns (busy waiting)
until the lock becomes free. Spinnlocks must be held for very short durations to avoid
wasing CPU and causing latency. If a interrupt happens on the same CPU that is currently
holding the spinlock it can cause a deadlock where the task holding the lock is stopped
by the interrupt running on the CPU meanwhile the interrupt waits for the lock to be
released. For that reason interrupts need to be disabled when holding a spinlock.
**Only disable interrups if the spinlock protects data shared with interrupt handlers
to avoid unnecesarily hurting latency!**

## Workqueues
Workqueues help running possible blocking functions from an atomic context by defer the
work, to execute it later in a proper context. Workqueues allow scheduling a function
in the future. The function will execute in a task context with lower priority than
interrupt handlers but higher priority than user processes.
