#keypoints
- the notion of a process
- features of processes
	- scheduling
	- creation
	- termination
	- comminucation
		- IPC
		- client-server systems


# Process Concept
a program in execution; process execution must progress in sequential fashion

## Process in Memory
- text section (program code)
- program counter (processor registers)
- stack -> temporary data (function parameters, return 
addresses, local variables)
- data section -> global variables
- heap -> dynamically allocated memory

## Process State

– new: The process is being created

– ready: The process is waiting to be assigned to a processor

– running: Instructions are being executed

– waiting: The process is waiting for some event to occur

– terminated: The process has finished execution

==relationship between states==

## Process Control Block (PCB)

==contents of PCB==

Context Switch


# Process Scheduling

Process scheduler selects among available processes for next execution on CPU and maintains scheduling queues.

## Scheduling Queues
– Job queue – set of all processes in the system
– Ready queue – set of all processes residing in main 
memory, ready and waiting to execute
– Device queues – set of processes waiting for an I/O device

## Schedulers
- Short-term scheduler (or CPU scheduler)
	- selects which process should be executed next and allocates CPU
	- Sometimes the only scheduler in a system
	- Short-term scheduler is invoked frequently (milliseconds) -> must be fast
- Long-term scheduler (or job scheduler)
	-  invoked infrequently (seconds, minutes)  -> may be slow)
	-  controls the degree of multiprogramming

 job pool (disk)

## Operations on Processes

### Process creation
Parent process create children processes -> a tree of processes
process identifier (pid)
systemd

### Process Termination
`exit()` system call
`abort()` system call (==reasons==)


# Inter-process Communication

Information sharing, communication speedup, modularity, convenience -> cooperating process -> need IPC

Two models
- message passing
  examples: **Mach**; **Windows XP**
- shared memory -> [[Synchronisation]]
   example: **Posix**

## Communications in Client-Server Systems

#q What models do they use?

### Socket
IP + Port + protocol to identify the unique process (service differentiation)
socket 161.25.19.8:1625 refers to port 1625 on host 161.25.19.8
All ports below 1024 are used for standard services
localhost 127.0.0.1 (loopback)

### Romote Procedure Calls (RPC)
分布式促使了RPC的诞生，RPC建立在Socket之上，again uses ports to identify process
**Stubs**
- client-side stub marshalls parameters (序列化，对象转为数据传输所需要的二进制格式，传给Server端)
- server-side stub unmarshalls parameters (反序列化为对象)

### Pipes

- Ordinary pipes
	- Usually **locally** not over a network
	- **Parent-child relationship**: A parent process creates a pipe and uses it to communicate with a child process that it created; cannot be accessed from outside this parent process
	- 过程：
		1. parent process调用pipe创建管道，并得到两个文件描述符：fd\[0\]指向读端，fd\[1\]指向写端；
		2. parent process调用fork创建child process，child process也有两个文件描述符指向同一管道；
		3. parent process关闭fd\[0\]，child process关闭fd\[1\]，从而前者写入管道，后者从管道读取，实现IPC -> **unidirectional**

- Named pipes
	- more powerful
	- can be accessed without a parent-child relationship
	- over network
	- bidirectional
	- can be used for several processes
	- provided both on UNIX and Windows systems

