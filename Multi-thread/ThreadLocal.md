# Thread Local

- Introduction
    We want to have separate instances(private copy) of a class so that there will not be any conflict among multiple threads. Each instance will be unique for each thread. This is nothing but a way of implementing thread safety.
    An important point about Thread Local variable is the global access. It can be accessed from anywhere inside the thread. Also note that, it is declared static and final.
- What is Thread Safe?
    A thread is a single line of process. When we refer multi-threaded applications, we mean that there multiple (sequential flow of control) line of process that runs through the same lines of code. In such situation, there is a possibility of one sequence(thread) accessing/modifying data of other sequence(thread). When data cannot be shared like this, then we make it Thread Safe. Following are the some of the different ways of implementing Thread Safe operation:
    
    1. Re-entrancy
    
    1. Mutual exclusion (synchronization)
    
    1. Thread-local
    
    1. Atomic operation
    
    So, in the above list Thread-local is one option. Hope now we get how Thread Local fits in the cube.
- Uses of Thread Local
    We have an object that is not Thread Safe and we want to use it safely. We go for synchronization by enclosing that object in synchronized block. Other way around is using Thread Safe, what it does is holds separate instance for each thread and makes it safe
