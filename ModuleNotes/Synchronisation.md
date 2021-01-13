#keypoints
- the concept of process synchronization
-  the critical-section problem
	- the consistency of shared data
	- software solutions
	- hardware solutions

Concurrent access to shared data may result in data inconsistency
-> mechanisms to ensure the orderly execution of cooperating processes

# Critical Section Problem

Each process has **critical section** segment of code
- Process may be changing common variables, updating 
table, writing file, etc
- When one process in critical section, no other may be in 
its critical section

Critical section problem is to design protocol to solve this

Each process must ask permission to enter critical section in 
entry section, may follow critical section with exit section, then 
remainder section

# Solution to Critical-Section Problem

三条件：
1. Mutual Exclusion (互斥) - If process $Pi$ is executing in its 
critical section, then no other processes can be 
executing in their critical sections
2. Progress (前进) - If no process is executing in its critical 
section and there exist some processes that wish to 
enter their critical section, then the selection of the 
processes that will enter the critical section next cannot 
be postponed indefinitely
3. Bounded Waiting (有限次等待) - A bound must exist on the number of times that other processes are allowed to enter their critical sections after a process has made a request to enter its critical section and before that request is granted

清华OS总结的三种方法：
1. 禁用硬件中断（仅限于单CPU）
2. 基于软件的解决方法
3. 高层抽象 -> **locking** -> 实现互斥

Kernel
- Preemptive: allows preemption of process when running in kernel mode
- Non-preemptive: runs until exits kernel mode, blocks, or voluntarily yields CPU; One process is active in the kernel at a time

## Peterson’s Solution
 two processes share two variables:
``` 
int turn;  
boolean flag[2];
```

变量 turn 表示哪个进程可以进入临界区；数组 flag 表示哪个进程准备进入临界区。
为了进入临界区，进程 Pi 首先设置 flag\[i\] 的值为 true；并且设置 turn 的值为 j，从而表示如果另一个进程 P<sub>j</sub>希望进入临界区，那么 P<sub>j</sub>能够进入。如果两个进程同时试图进入，那么 turn 会几乎在同时设置成 i 和 j。只有一个赋值语句的结果会保持；另一个也会设置，但会立即被重写。变量 turn 的最终值决定了哪个进程允许先进入临界区。

==证明三条件成立==

Peterson’s algorithm doesn’t always work on modern 
computer architectures with several CPUs as events might 
be reordered.

## Synchronisation Hardware

Modern machines provide special **atomic** (non-interruptible) hardware instructions
通过硬件逻辑保证原子操作
- 适用于单CPU，也支持了共享主存的多CPU中任意数量的进程
- 可以支持多临界区
- 开销小

Main Alogorithms:
- test memory word and set value
- swap contents of two memory words

### Solution using test_and_set()
Shared Boolean variable `lock`, initialised to `FALSE`
获取内存单元的值，使用一个临时变量储存该值，返回这个变量，并将内存单元的值设为`True`

### Solution using compare_and_swap()
Shared integer `lock` initialized to `0`

Although the two algorithms (test_and_set() and compare_and_swap()) satisfy the mutual-exclusion 
requirement, they **do not satisfy the bounded-waiting 
requirement**.

这两种功能相似的硬件指令都可用来实现锁，可以忙等待也可以无忙等待，适合使用者保持锁时间较短（临界区短）的情况

可能存在的问题：
- 死锁
	- 低优先级进程获得lock，高优先级进程占用CPU，lock无法释放
- while循环做判断时会抢lock（该过程比较随机），因此进程离开临界区且多个进程在等待时可能导致饥饿
- while循环是busy waiting，线程等待时wasted CPU cycles，过多占用CPU资源 -> 结合进程管理的内容([[Processes#Context Switch]])，令其睡眠，挂入waiting queue，从而实现非忙等待


### Mutex Locks

`acquire()` and `release()` atomic (Usually implemented via hardware atomic instructions)

This solution requires busy waiting, therefore called a spinlock 自旋锁

### Semaphore 信号量
一个integer variable，Semaphore `S`
两个原子操作，`wait()` and `signal()`（Originally called `P()`，`V()`）
#q 这里的具体定义的细节指令顺序和清华课程不同

With each semaphore there is an associated waiting queue
Each entry in a waiting queue has two data items:
1. value (of type integer)
2. pointer to next record in the list

Two operations:
- block – place the process invoking the operation on the appropriate waiting queue
- wakeup – remove one of processes in the waiting queue and place it in the ready queue