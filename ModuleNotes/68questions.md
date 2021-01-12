## Lecture1:
1. Von Neumann Architecture:five major components
## Lecture2:
1. How does Fetch-Decode-Execute work?
2. LMC(https://www.vivaxsolutions.com/web/lmc.aspx): 
- Insturction set: 1xx,2xx,3xx,5xx,6xx,7xx,8xx,901,902,000,**DAT**
- example:
    - INP
    - STA A
    - OUT
    - HLT
    - A    DAT

## Lecture11:
an overview of the sequence of operations the computer executes at start-up and the main components involved in this phase.
1. Essentials steps after turning on a computer:
- Boot loader(holdering minimal things to do before os) load settings from CMOS
- Initialize hardware(may have their own BIOS)and run POST
- Bootstrapping: find MBR with bootloader in it, then launch the os.
Q: why do we separate BIOS and CMOS into two chips?
Q: why do we put os in main memory rather than disk?



## Lecture14:
1. To introduce the notion of a process--a program in execution, which forms the basis of all computation
- How does a process will be presented in memory(stack, heap, data, text)?
- How does the state of a process change in a lifecycle?
- What are the components of a PCB, or the info related?
- What is context switch?(save & reload)
2. To describe the various features of processes, including scheduling, creation and termination, and communication
- What are scheduling queues and scheduler? how does short term scheduler compare to long one?
- How does the creation and termination of processes work?
- Example, Chrome is multi process(browser, render, plug-in)
3. To explore inter-process communication using shared memory and message passing
- What are two models of IPC? windows&, mach (message passing)
4. To describe communication in client-server systems
- What are three communication methods for client-server system?
- How does socket, remote procedure call & pip(ordinary, named) work?

## Lecture15:
1. The concept for process synchronization
- concurrent access to shared data might lead to data inconsistency
2. Introduce the critical-section problem, whose solutions can be used to ensure the consistency of shared data
- To design protocol to allow process enter&exit crticial section.
- How should the solution look like?(mutual exclusion, progress(remainder-waiting), bounded waiting)
- How does preemptive&non-preemptive approach mean?
3. Both software and hardware solutions of the critical-section problem
- How does the Peter's solution work?(turn,flag),for one cpu system. 
- How does locking work in test/set, compare/swap?
- why does the two algo above not satisfy the bounded waiting and how to solve that?
- How does mutex work and why semaphore is better?
- why busy wating is a problem? wasted cpu cycles.
- How to improve semaphore with block and wakeup so that there is no busy waiting?

## Lecture16:
1. Introduce CPU scheduling, which is the basis for multiprogrammed operating systems
- what is a cycle of process execution?
- when do you need to schedule a process? 4 scenarios
- when is the scheduling preemptive or non-preemptive?
- what does dispatcher do and what is dispatcher latency?
2. To discuss evaluation criteria for selecting a CPU-scheduling algorithm for a particular system
- what are the scheduling criteria?
- To what extent should we keep cpu busy?(4-9)?
3. To describe various CPU-scheduling algorithms
- what is FCFS and what is the convoy effect?
- what is SJF(nonpreemptive)and how to predict the length of next CPU burst? what is exponential averaging?
- **Example of preemptive SJF**
- what is the problem with Priority Scheduling and solution to that?(starvation)
- what is Round Robin and what consideration should be taken into account for setting a q(time quantum)?
- How does accounting time of process will help determine optimal q?
- what is multilevel queue scheduling and how to improve it with feedback?
- why study all the algorithms rather than the best one?different computing environment
4. To examine the scheduling algorithms of some operating systems
- what is the algo in windows?

Summary: 
- two main factors: number of queues and algo for each of them
- use historical data to determine optimal solution

## Lecture17:
• To provide a detailed description of various ways of organising memory.
- What's the difference between logical and physical memory
- What is used to define logical address space? base limit(used for hardware protection)
- what are the stages that a binding of instruction and data to memory can happen? and which one is most common?(execution time)
- What does MMU do?Map from logical to physcial ... What does Swapping do? extend physical space...

• To explore various techniques of allocating memory to processes .
- **contiguous allocation**
- multiple-partition allocation
- dynamic storage allocaiton - best first, worst fit, first fit
- what are two types of fragmentation(empty holes)? and solution to them?(compaction)
- what are two techniques in **noncontiguous allocation**?

• To discuss how segmentation and paging work in modern systems.
- how does segmentation work?architecture(table, segment-number, offset)
- How does the mapping from logical space to physical space work for paging?(p,d)
- How to calculate fragmentation size based on page size & process size?
- Why do we need two types of page size for kernels? 
- What are the differences between segmentation and paging?

## Lecture18:
• To describe the benefits of a virtual memory system.
- advantages? - more logical memory, partially loaded program -> more concurrent programming, less I/O

• To explain the concepts of demand paging.
- Steps in handling page fault(trap,swap,i-v,restart)
- locality of reference?
- what is pure demand paging?
- How does COPYONWRITE allow more efficient process creation?

• To detail and compare few page replacement algorithms.
- basic page replacement: using modify bit(dirty bit) to put back a victim frame
- aim of algorithms: reduce page faults
- How does FIFO work? what is 'belday's anomaly'? and why we call an optimal solution 'orcale'?
- How does LastRecentlyUsed (LRU) work? how does the two implementation compare to each other?

## Lecture19:
Parameters of hard disk: size, storate, transfer rate, effective transfer rate, seek time, spindle speed
nonvolatile memeory devices: if like disk, then SSD(more reliable, faster, less capacity, no latency)
how does the disk attach to the cpu?(I/O buses, controller)
Address mapping: 1-D array mapped to sectors from outside to inside. Constant angular speed --> inner sider more data
Dis scheduling: improve access time and bandwidth by managing the order in which storage I/O requests are needed.
- FCFS:
- SSTF:236
- SCAN: 236 if heavies density on the other side, then long waits.
- C-SCAN: one go to the end.
- C-LOOK: 153, stop at largest request

