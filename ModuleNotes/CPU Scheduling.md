#keypoints 
-  CPU scheduling as basis for multiprogrammed operating systems
-  CPU-scheduling algorithms
-  evaluation criteria for selecting a CPU-scheduling algorithm for a particular system
- #q examine the scheduling algorithms of some operating systems

# CPU Scheduling Concept
- Maximum CPU utilization obtained with multiprogramming
- CPU–I/O Burst Cycle: Process execution consists of a cycle of CPU execution and I/O wait
- CPU burst followed by I/O burst
- CPU burst distribution is of main concern

CPU burst: the amount of time the process uses the processor to complete

进程执行由 CPU 执行和 I/O 等待 周期（cycle） 组成，进程在这两个状态之间切换。进程执行从 CPU区间（CPU burst） 开始，之后在 CPU 区间和 I/O 区间 交换

# CPU Scheduler
**Short-term scheduler** selects from among the processes in **ready queue**, and allocates the CPU to one of them (Queue may be ordered in various ways)

CPU scheduling decisions may take place when a process:
1. Switches from running to waiting state
2. Switches from running to ready state
3. Switches from waiting to ready
4. Terminates

**Scheduling under 1 and 4 is nonpreemptive(不可抢占); all other scheduling is preemptive(可以抢占)**
- Consider access to shared data
- Consider interrupts occurring during crucial OS activities

## Dispatcher
Dispatcher module gives control of the CPU to the process 
selected by the short-term scheduler; this involves:
- switching context
- switching to user mode
- jumping to the proper location in the user program to restart that program

**Dispatch latency** – time it takes for the dispatcher to stop one process and start another running


# Scheduling Algorithm

## Optimisation Criteria
- Max CPU utilisation
	- keep the CPU as busy as possible
- Max throughput 吞吐量，单位时间内成功地传送数据的数量
	- \# of processes that complete their execution per time unit
- Min turnaround time
	- amount of time to execute a particular process
- Min waiting time
	- amount of time a process has been waiting in the ready queue
- Min response time
	- amount of time it takes from when a request was submitted until the first response is produced, not output (for time-sharing environment)

1. We are going to use the **average waiting time** to compare the following algorithms.
2. We consider only one CPU burst (in milliseconds) per process.

## First- Come, First-Served (FCFS) Scheduling
- 最简单的 CPU 调度算法，先请求 CPU 的进程先分配到 CPU，该算法可用 FIFO 队列实现，且**平均等待时间通常较长**
-  non-preemptive -> 不适合分时系统（使一台计算机采用时间片轮转的方式同时为几个、几十个甚至几百个用户服务的一种操作系统）
	-  允许一个进程保持过长的 CPU 时间是非常严重的错误，多个进程等待一个长进程释放 CPU的现象（short process behind long process）称为convoy effect（护航效果），如系统中存在一个 CPU 约束进程（长）和多个 I/O 约束进程（短）
	- 通过burst time可以计算average waiting time

## Shortest-Job-First (SJF) Scheduling
- Associate with each process the length of its next CPU burst， use these lengths to schedule the process with the shortest time
	- 如果有多个进程的下一次 CPU 区间长度相同，则在这些进程间使用 FCFS
- SJF is optimal – gives minimum average waiting time for a given set of processes，证明可考虑贪心算法
- The difficulty is knowing the length of the next CPU request
	- 通过先验长度预测下一个 CPU burst length [[#Determining Length of Next CPU Burst]]
- There are two versions of the algorithm: non-preemptive and preemptive known as **shortest-remaining-time-first**((最短剩余时间优先)
	- 对于抢占 SJF 调度算法，当一个新进程到达就绪队列且当前正在执行的进程剩余时间比新进程所需 CPU 时间长，则新进程抢占 CPU
	
### Determining Length of Next CPU Burst
Can only estimate the length, it should be similar to the previous one -> **exponential averaging**

Computation:
1. $t_{n}=$ actual length of $n^{th}$ CPU burst
2. $\tau_{n+1}=$ predicted value for the next CPU burst
3.  $0 \leq \alpha \leq 1$，参数 $\alpha$ 控制了最近和过去历史在预测中的相对加权
4.  $\tau_{n+1}=\alpha t_{n} + (1-\alpha)\tau_{n}$

$\alpha$ is the smoothing factor, commonly set to 1/2.

### Priority Scheduling
- A priority number (integer) is associated with each process
- The CPU is allocated to the process with the highest priority (smallest integer ≡ highest priority or opposite)
	- Preemptive
	- Nonpreemptive
- SJF is priority scheduling where priority is the inverse of predicted next CPU burst time
- Problem ≡ Starvation – low priority processes may never execute
	- Solution ≡ Aging 老化 – as time progresses increase the priority of the process

### Round Robin (RR) 轮转法
- 专门为分时系统设计，类似 FCFS 调度，但强制通过抢占切换进程
- Each process gets a small unit of CPU time (**time quantum** q), usually 10-100 milliseconds. After this time has elapsed, the process is preempted and added to the end of the ready queue 就绪队列是环形链表，并保持 FIFO 性质， 新到达的进程会被添加到队列末尾
- If there are n processes in the ready queue and the time quantum is q, then each process gets 1/n of the CPU time in chunks of at most q time units at once. No process waits more than (n-1)q time units.
- **Timer** interrupts every quantum to schedule next process
- Typically, higher average turnaround than SJF, but better response

#### Time Quantum and Context Switch Time
- q large 等价于 FCFS (First Come First Served)
- q small -> q must be large with respect to context switch, otherwise overhead is too high
	- q usually 10ms to 100ms, context switch < 10 µsec

### Multilevel Queue Scheduling 多级队列调度
适用于进程容易分组的情况，如划分为 前台（交互） 进程和 后台（批处理） 进程
- Ready queue is partitioned into separate queues, eg: foreground (interactive)； background (batch)
	- Process permanently in a given queue
	- Each queue has its own scheduling algorithm:
		- foreground – RR
		- background – FCFS
- Scheduling must be done between the queues
	- Fixed priority scheduling
		- i.e., serve all from foreground then from background)
		- Possibility of starvation
		- real-time processes > system processes > interactive processes > batch processes
		- 系统进程 > 交互进程 > 交互编辑进程 > 批处理进程
	- Time slice
		- each queue gets a certain amount of CPU time which it can schedule amongst its processes; i.e., 80% to foreground in RR, 20% to background in FCFS

#### Multilevel Feedback-Queue Scheduling
A process can move between the various queues; aging can be 
implemented this way

Three queues: 
- $Q_{0}$ – RR with time quantum 8 milliseconds
- $Q_{1}$ – RR time quantum 16 milliseconds
- $Q_{2}$ – FCFS

- A new job enters queue $Q_{0}$ . It receives 8 milliseconds
- If it does not finish in 8 milliseconds, job is moved to queue $Q_{1}$ and receives 16 additional milliseconds
- If it still does not complete, it is preempted and moved to queue $Q_{2}$  ans scheduled using FCFS

### Windows Scheduling
- The scheduler is called [[#Dispatcher]] in Windows
- Windows NT-based versions uses **priority-based [[#Multilevel Feedback-Queue Scheduling]]**
	- Highest-priority process runs next
	- Process runs until (1) blocks, (2) uses time slice, (3) preempted by higher-priority process
	- 32-level priority scheme 0 to 31
	- Queue for each priority
	- If no run-able process, runs system idle process
