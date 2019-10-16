# 进程和线程(Process vs Thread)

## 进程
    1. 定义
    
    进程是一个具有一定独立功能的程序关于某个数据集合的一次运行活动。它是操作系统动态执行的基本单元，在传统的操作系统中，进程既是基本的分配单元，也是基本的执行单元。
    进程是具有独立功能的程序关于某个数据集合上的一次运行活动，是系统进行资源分配和调度（若不支持线程机制，进程的系统调度的单位。否则，线程是系统调度的单位）的独立单位。
    　
    特点
    
        进程是程序的一次执行过程。若程序执行两次甚至多次，则需要两个甚至多个进程。
        进程是是正在运行程序的抽象。它代表运行的CPU，也称进程是对CPU的抽象。（虚拟技术的支持，将一个CPU变幻为多个虚拟的CPU）
        系统资源（如内存、文件）以进程为单位分配。
        操作系统为每个进程分配了独立的地址空间
        操作系统通过“调度”把控制权交给进程。
    
- What is Thread Safe?
    A thread is a single line of process. When we refer multi-threaded applications, we mean that there multiple (sequential flow of control) line of process that runs through the same lines of code. In such situation, there is a possibility of one sequence(thread) accessing/modifying data of other sequence(thread). When data cannot be shared like this, then we make it Thread Safe. Following are the some of the different ways of implementing Thread Safe operation:
    
    1. Re-entrancy
    
    1. Mutual exclusion (synchronization)
    
    1. Thread-local
    
    1. Atomic operation
    
    So, in the above list Thread-local is one option. Hope now we get how Thread Local fits in the cube.
- Uses of Thread Local
    We have an object that is not Thread Safe and we want to use it safely. We go for synchronization by enclosing that object in synchronized block. Other way around is using Thread Safe, what it does is holds separate instance for each thread and makes it safe
