# Implementing Synchronization

Three phases of a synchronization event
1. Acquire method
    - How a thread attempts to gain
    access to protected resource
2. Waiting algorithm
    - How a thread waits for access to be granted to shared resource
3. Release method
    - How thread enables other threads to gain resource when its 
work in the synchronized region is complete



- Busy waiting (a.k.a. “spinning”)
```
while (condition X not true) {}
logic that assumes X is true
```

- Blocking synchronization: 
if progress cannot be made because a resource cannot 
be acquired, it is desirable to free up execution resources for 
another thread (preempt the running thread)

