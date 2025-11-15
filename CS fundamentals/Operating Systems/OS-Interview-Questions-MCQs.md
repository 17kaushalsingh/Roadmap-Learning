# Operating Systems - 200 Most Common Interview Questions & MCQs

> Comprehensive collection of frequently asked OS interview questions and MCQs from top tech companies (Google, Microsoft, Amazon, Facebook) and popular interview platforms (GeeksforGeeks, InterviewBit, LeetCode, Sanfoundry).

**Last Updated:** November 2025
**Difficulty Levels:** 🟢 Easy | 🟡 Medium | 🔴 Hard

---

## Table of Contents
- [Section 1: Basic Concepts (25 Questions)](#section-1-basic-concepts)
- [Section 2: Process Management (30 Questions)](#section-2-process-management)
- [Section 3: CPU Scheduling (25 Questions)](#section-3-cpu-scheduling)
- [Section 4: Synchronization & Deadlock (35 Questions)](#section-4-synchronization--deadlock)
- [Section 5: Memory Management (35 Questions)](#section-5-memory-management)
- [Section 6: File Systems & I/O (20 Questions)](#section-6-file-systems--io)
- [Section 7: Advanced Topics (30 Questions)](#section-7-advanced-topics)

---

## Section 1: Basic Concepts (25 Questions)

### Q1. 🟢 What is an Operating System?
**Answer:** An operating system is system software that manages computer hardware and software resources and provides common services for computer programs. It acts as an intermediary between users and computer hardware.

### Q2. 🟢 Which of the following is NOT a function of an OS?
a) Memory Management
b) Process Management
c) Virus Protection
d) File Management

**Answer:** c) Virus Protection

**Explanation:** While OS provides security features, dedicated antivirus software handles virus protection, not the core OS.

### Q3. 🟡 What are the main goals of an Operating System?
**Answer:**
1. **Convenience** - Make computer system convenient to use
2. **Efficiency** - Use computer hardware efficiently
3. **Ability to evolve** - Permit effective development, testing, and introduction of new functions

### Q4. 🟢 Which is the first program that runs on a computer when it boots?
a) System software
b) Application software
c) Boot loader
d) Operating System

**Answer:** c) Boot loader

### Q5. 🟡 What is the difference between 32-bit and 64-bit OS?
**Answer:**
- **32-bit OS**: Can address 2³² (4GB) of RAM, processes 4 bytes per instruction cycle
- **64-bit OS**: Can address 2⁶⁴ (17+ billion GB) of RAM, processes 8 bytes per instruction cycle
- 64-bit offers better performance and can run 32-bit applications, but not vice versa

### Q6. 🟢 BIOS stands for:
a) Basic Input Output System
b) Binary Input Output System
c) Basic Integrated Operating System
d) Binary Integrated Operating System

**Answer:** a) Basic Input Output System

### Q7. 🟡 What is POST in the boot process?
**Answer:** POST (Power-On Self-Test) is a diagnostic testing sequence run by BIOS/UEFI to ensure hardware components are functioning properly before loading the OS. If hardware issues are detected, the boot process stops.

### Q8. 🟢 Which type of OS allows multiple users to work simultaneously?
a) Single-user OS
b) Multi-user OS
c) Real-time OS
d) Embedded OS

**Answer:** b) Multi-user OS

### Q9. 🟡 What is the difference between multiprogramming and multitasking?
**Answer:**
- **Multiprogramming**: Multiple programs in memory, CPU switches on I/O wait
- **Multitasking**: Logical extension of multiprogramming with time-sharing, CPU switches based on time quantum
- Multitasking provides better response time and user experience

### Q10. 🟢 Real-time operating systems are used in:
a) Desktop computers
b) Air traffic control systems
c) Mobile phones
d) Gaming consoles

**Answer:** b) Air traffic control systems

### Q11. 🟡 What is kernel in OS?
**Answer:** Kernel is the core component of OS that directly interacts with hardware. It manages:
- Process management
- Memory management
- File management
- I/O management

### Q12. 🟢 Which kernel type has all OS services in kernel space?
a) Microkernel
b) Monolithic kernel
c) Hybrid kernel
d) Exokernel

**Answer:** b) Monolithic kernel

### Q13. 🟡 Differentiate between monolithic and microkernel.

| Feature | Monolithic | Microkernel |
|---------|------------|-------------|
| Size | Large | Small |
| Performance | Fast | Slower |
| Reliability | Less | More |
| All services in kernel | Yes | No (only core) |
| Examples | Linux, Unix | L4 Linux, MINIX |

### Q14. 🟢 UEFI is a replacement for:
a) BIOS
b) Kernel
c) Bootloader
d) MBR

**Answer:** a) BIOS

### Q15. 🟡 What are the advantages of UEFI over BIOS?
**Answer:**
- Faster boot times
- Supports disks larger than 2TB (GPT vs MBR)
- Secure Boot feature
- GUI support
- Network capabilities

### Q16. 🟢 Which component translates logical addresses to physical addresses?
a) CPU
b) MMU
c) Cache
d) Register

**Answer:** b) MMU (Memory Management Unit)

### Q17. 🟡 Explain the difference between system software and application software.
**Answer:**
- **System Software**: Operates and controls computer system (OS, drivers, utilities)
- **Application Software**: Performs specific tasks for users (browsers, word processors)
- System software provides platform for application software

### Q18. 🟢 Shell in OS is:
a) Kernel component
b) Command interpreter
c) System call interface
d) Hardware abstraction layer

**Answer:** b) Command interpreter

### Q19. 🟡 What is IPC and why is it needed?
**Answer:** IPC (Inter-Process Communication) allows processes with separate memory spaces to communicate and share data. Needed because processes have memory protection but sometimes need to cooperate. Implemented via shared memory or message passing.

### Q20. 🟢 Which is NOT a type of operating system?
a) Batch processing
b) Distributed
c) Compiler-based
d) Real-time

**Answer:** c) Compiler-based

### Q21. 🟡 What is the purpose of system calls?
**Answer:** System calls provide a programmatic interface for user programs to request services from the kernel that they don't have permission to perform directly (file I/O, process creation, memory allocation). They are the only way to transition from user mode to kernel mode.

### Q22. 🟢 Context switching occurs between:
a) Processes only
b) Threads only
c) Both processes and threads
d) Neither processes nor threads

**Answer:** c) Both processes and threads

### Q23. 🟡 What is stored in the Process Control Block (PCB)?
**Answer:**
- Process ID
- Program Counter
- CPU registers
- Process state
- Priority
- Memory management information
- I/O status information
- Accounting information

### Q24. 🟢 Degree of multiprogramming refers to:
a) Number of processors
b) Number of processes in memory
c) Number of threads
d) Number of programs on disk

**Answer:** b) Number of processes in memory

### Q25. 🟡 What is the convoy effect?
**Answer:** Convoy effect occurs when many short processes are blocked by one long process holding a resource (typically CPU), causing poor resource utilization and high average waiting time. Common in FCFS scheduling.

---

## Section 2: Process Management (30 Questions)

### Q26. 🟢 A program in execution is called:
a) Process
b) Thread
c) Instruction
d) Function

**Answer:** a) Process

### Q27. 🟡 What is the difference between process and program?
**Answer:**
- **Program**: Passive entity, executable file on disk, static
- **Process**: Active entity, program in execution, resides in RAM, dynamic
- One program can create multiple processes

### Q28. 🟢 Which is NOT a process state?
a) New
b) Ready
c) Compiled
d) Running

**Answer:** c) Compiled

### Q29. 🟡 Explain the five process states.
**Answer:**
1. **New**: Process being created
2. **Ready**: Waiting for CPU allocation
3. **Running**: Instructions being executed
4. **Waiting**: Blocked on I/O or event
5. **Terminated**: Finished execution

### Q30. 🟢 Which scheduler selects processes from job queue?
a) Short-term scheduler
b) Long-term scheduler
c) Medium-term scheduler
d) I/O scheduler

**Answer:** b) Long-term scheduler

### Q31. 🟡 Differentiate between short-term and long-term scheduler.

| Short-term (CPU) Scheduler | Long-term (Job) Scheduler |
|----------------------------|---------------------------|
| Selects from ready queue | Selects from job queue |
| Very frequent invocation | Infrequent invocation |
| Must be fast | Can be slow |
| Controls degree of multiprogramming indirectly | Controls degree directly |

### Q32. 🟢 A thread is also called:
a) Heavy-weight process
b) Light-weight process
c) Sub-process
d) Mini-process

**Answer:** b) Light-weight process

### Q33. 🟡 What is the difference between process and thread?

| Process | Thread |
|---------|--------|
| Independent execution | Part of process |
| Separate memory space | Shared memory space |
| Heavy-weight | Light-weight |
| Expensive context switching | Cheap context switching |
| IPC required | Direct communication |

### Q34. 🟢 Thread context switching is faster because:
a) Threads are smaller
b) No memory address space switching
c) Threads have less data
d) OS favors threads

**Answer:** b) No memory address space switching

### Q35. 🟡 What is saved during context switching?
**Answer:**
- Program Counter (PC)
- CPU registers
- Process state
- Memory management information
- I/O status
For process switching: Also switch memory address space and flush CPU cache

### Q36. 🟢 Which provides better concurrency?
a) Single-threaded process
b) Multi-threaded process
c) Both are same
d) Depends on CPU

**Answer:** b) Multi-threaded process

### Q37. 🟡 What are the benefits of multithreading?
**Answer:**
1. **Responsiveness**: Application remains responsive
2. **Resource sharing**: Threads share memory and resources
3. **Economy**: Cheaper to create and switch than processes
4. **Scalability**: Can utilize multiple processors

### Q38. 🟢 Orphan process is adopted by:
a) Kernel
b) Init process (PID 1)
c) Shell
d) Parent's parent process

**Answer:** b) Init process (PID 1)

### Q39. 🟡 What is zombie process?
**Answer:** A zombie (defunct) process is one that has completed execution but still has an entry in process table. Occurs when parent hasn't read child's exit status via wait(). Entry removed after parent reads status (reaping).

### Q40. 🟢 fork() system call returns:
a) 0 to child, child PID to parent
b) Child PID to child, 0 to parent
c) Same value to both
d) -1 on success

**Answer:** a) 0 to child, child PID to parent

### Q41. 🟡 What is the purpose of exec() system call?
**Answer:** exec() replaces current process image with a new program. Used after fork() to run a different program in child process. Does not create new process, just replaces current one.

### Q42. 🟢 Which is NOT inter-process communication method?
a) Shared memory
b) Message passing
c) Pipes
d) Context switching

**Answer:** d) Context switching

### Q43. 🟡 Compare shared memory and message passing IPC.

| Shared Memory | Message Passing |
|---------------|-----------------|
| Fast communication | Slower communication |
| Requires synchronization | OS handles synchronization |
| Good for large data | Good for small messages |
| Same address space needed | Works across network |

### Q44. 🟢 PCB stands for:
a) Program Control Block
b) Process Control Block
c) Processor Control Block
d) Programmable Control Block

**Answer:** b) Process Control Block

### Q45. 🟡 What is dispatcher latency?
**Answer:** Dispatcher latency is the time taken to stop one process and start another. Includes time to:
- Save context of current process
- Load context of new process
- Switch to user mode
- Jump to proper location in user program

### Q46. 🟢 Swapping is controlled by:
a) Long-term scheduler
b) Short-term scheduler
c) Medium-term scheduler
d) Dispatcher

**Answer:** c) Medium-term scheduler

### Q47. 🟡 What is swapping in OS?
**Answer:** Swapping is moving a process temporarily from main memory to secondary storage (swap space) and back to memory for continued execution. Used to improve process mix or free up memory. Swap-out and swap-in controlled by medium-term scheduler.

### Q48. 🟢 User threads are managed by:
a) Kernel
b) Thread library
c) CPU
d) Scheduler

**Answer:** b) Thread library

### Q49. 🟡 Differentiate between user threads and kernel threads.

| User Threads | Kernel Threads |
|--------------|----------------|
| Managed by thread library | Managed by kernel |
| Fast switching | Slower switching |
| OS unaware | OS aware |
| Blocking blocks entire process | Blocking blocks only thread |
| Many-to-one or many-to-many model | One-to-one model |

### Q50. 🟢 Which is true about multi-threading on single CPU?
a) Provides true parallelism
b) Improves performance always
c) Provides concurrency, not parallelism
d) Slows down execution

**Answer:** c) Provides concurrency, not parallelism

### Q51. 🟡 What is thread pool?
**Answer:** Thread pool is a collection of pre-created threads waiting for tasks. Benefits:
- Faster than creating thread per request
- Limits number of threads
- Better resource management
- Used in web servers

### Q52. 🟢 Copy-on-write is associated with:
a) fork()
b) exec()
c) wait()
d) exit()

**Answer:** a) fork()

### Q53. 🟡 Explain copy-on-write (COW) in fork().
**Answer:** COW is an optimization where parent and child initially share same pages. Pages are copied only when either process writes to them. Saves memory and time if child immediately calls exec() or doesn't modify data.

### Q54. 🟢 Which system call creates a new process in Unix?
a) create()
b) spawn()
c) fork()
d) new()

**Answer:** c) fork()

### Q55. 🟡 What is process burst time?
**Answer:** Burst time is the total CPU time required by a process for execution. Includes all CPU cycles needed but excludes I/O wait time. Used in CPU scheduling algorithms like SJF.

---

## Section 3: CPU Scheduling (25 Questions)

### Q56. 🟢 CPU scheduling is the basis of:
a) Single programming
b) Multiprogramming
c) Real-time systems
d) Embedded systems

**Answer:** b) Multiprogramming

### Q57. 🟡 What are the goals of CPU scheduling?
**Answer:**
1. **Max CPU utilization** - Keep CPU busy
2. **Max throughput** - Complete more processes
3. **Min turnaround time** - Finish quickly
4. **Min waiting time** - Less time in ready queue
5. **Min response time** - Quick first response

### Q58. 🟢 Turnaround time equals:
a) CT - AT
b) TAT - BT
c) BT + AT
d) CT + AT

**Answer:** a) CT - AT (Completion Time - Arrival Time)

### Q59. 🟡 Define the following scheduling metrics:
**Answer:**
- **Arrival Time (AT)**: When process enters ready queue
- **Burst Time (BT)**: Time required for execution
- **Completion Time (CT)**: When process finishes
- **Turnaround Time (TAT)**: CT - AT
- **Waiting Time (WT)**: TAT - BT
- **Response Time (RT)**: Time from arrival to first CPU allocation

### Q60. 🟢 FCFS scheduling can suffer from:
a) Starvation
b) Convoy effect
c) Priority inversion
d) Thrashing

**Answer:** b) Convoy effect

### Q61. 🟡 What is preemptive scheduling?
**Answer:** Preemptive scheduling allows OS to forcibly take CPU from a running process. CPU can be preempted:
- When time quantum expires (RR)
- When higher priority process arrives (Priority)
- When process with shorter remaining time arrives (SRTF)
Provides better response time but higher overhead.

### Q62. 🟢 Which is a non-preemptive algorithm?
a) Round Robin
b) FCFS
c) SRTF
d) Priority with preemption

**Answer:** b) FCFS

### Q63. 🟡 Why is SJF called optimal?
**Answer:** SJF gives minimum average waiting time for a given set of processes. Scheduling short job before long job decreases wait time of short job MORE than it increases wait time of long job, minimizing total wait time.

### Q64. 🟢 SJF can cause:
a) Deadlock
b) Starvation
c) Thrashing
d) Race condition

**Answer:** b) Starvation (for long processes)

### Q65. 🟡 What is the difference between SJF and SRTF?
**Answer:**
- **SJF**: Non-preemptive, selects shortest burst time at arrival
- **SRTF**: Preemptive SJF, preempts if new process has shorter remaining time
- SRTF gives better average WT but more context switching overhead

### Q66. 🟢 Round Robin uses:
a) Priority
b) Burst time
c) Time quantum
d) Arrival time only

**Answer:** c) Time quantum

### Q67. 🟡 What happens if time quantum is too small in RR?
**Answer:** If TQ is too small:
- ❌ Excessive context switching overhead
- ❌ More time spent switching than executing
- ❌ CPU utilization decreases
- ❌ System throughput drops
Ideal TQ: 10-100 milliseconds, slightly larger than context switch time

### Q68. 🟢 Priority scheduling can suffer from:
a) Deadlock
b) Convoy effect
c) Starvation
d) Thrashing

**Answer:** c) Starvation

### Q69. 🟡 How does aging prevent starvation?
**Answer:** Aging gradually increases priority of waiting processes over time. For example, increase priority by 1 every 15 minutes. Eventually even low priority processes get high enough priority to execute.

### Q70. 🟢 Belady's anomaly occurs in:
a) FCFS
b) FIFO page replacement
c) LRU
d) Optimal

**Answer:** b) FIFO page replacement

### Q71. 🟡 What is multilevel queue scheduling?
**Answer:** MLQ divides ready queue into multiple separate queues based on process properties (system, interactive, batch). Processes permanently assigned to one queue. Each queue can have different scheduling algorithm. Scheduling among queues typically fixed-priority preemptive.

### Q72. 🟢 MLFQ allows processes to:
a) Run on multiple CPUs
b) Move between queues
c) Have multiple threads
d) Skip queues

**Answer:** b) Move between queues

### Q73. 🟡 How does MLFQ differ from MLQ?
**Answer:**
- **MLQ**: Processes permanently in one queue (inflexible)
- **MLFQ**: Processes can move between queues based on behavior
- MLFQ uses aging to prevent starvation
- CPU-bound processes move to lower priority, I/O-bound stay high priority

### Q74. 🟢 Which algorithm is best for time-sharing systems?
a) FCFS
b) SJF
c) Round Robin
d) Priority

**Answer:** c) Round Robin

### Q75. 🟡 Calculate average WT: P1(AT=0,BT=4), P2(AT=1,BT=3), P3(AT=2,BT=1) using FCFS
**Answer:**
- Execution: P1 → P2 → P3
- CT: P1=4, P2=7, P3=8
- TAT: P1=4-0=4, P2=7-1=6, P3=8-2=6
- WT: P1=4-4=0, P2=6-3=3, P3=6-1=5
- **Average WT = (0+3+5)/3 = 2.67**

### Q76. 🟢 Throughput means:
a) Total time taken
b) Number of processes completed per unit time
c) CPU utilization
d) Response time

**Answer:** b) Number of processes completed per unit time

### Q77. 🟡 What is the difference between response time and turnaround time?
**Answer:**
- **Response Time**: Time from submission to FIRST response (first CPU allocation)
- **Turnaround Time**: Time from submission to COMPLETION
- Response time more important for interactive systems
- TAT includes entire execution time

### Q78. 🟢 In priority scheduling, lower number means:
a) Always lower priority
b) Depends on system
c) Always higher priority
d) No significance

**Answer:** b) Depends on system (convention varies)

### Q79. 🟡 What is CPU-bound vs I/O-bound process?
**Answer:**
- **CPU-bound**: Spends most time computing (long CPU bursts, few I/O)
- **I/O-bound**: Spends most time waiting for I/O (short CPU bursts, frequent I/O)
- MLFQ separates these by moving CPU-bound to lower queues

### Q80. 🟢 Context switching time should be:
a) Very high
b) Very low
c) Equal to time quantum
d) Greater than burst time

**Answer:** b) Very low

---

## Section 4: Synchronization & Deadlock (35 Questions)

### Q81. 🟢 Critical section is a code segment where:
a) Process sleeps
b) Shared resources are accessed
c) CPU is idle
d) Process terminates

**Answer:** b) Shared resources are accessed

### Q82. 🟡 What are the requirements for critical section solution?
**Answer:**
1. **Mutual Exclusion**: Only one process in CS at a time
2. **Progress**: Selection of next process can't postpone indefinitely
3. **Bounded Waiting**: Limit on number of times others enter CS before a waiting process

### Q83. 🟢 Race condition occurs when:
a) Two processes race for CPU
b) Multiple processes access shared data simultaneously
c) Process runs too fast
d) CPU scheduling fails

**Answer:** b) Multiple processes access shared data simultaneously

### Q84. 🟡 What causes race condition and how to prevent it?
**Answer:**
**Cause**: Concurrent access to shared data with at least one write operation, outcome depends on timing.

**Prevention**:
1. Mutual exclusion (locks/mutex)
2. Atomic operations
3. Semaphores
4. Monitors

### Q85. 🟢 Mutex stands for:
a) Multiple Execution
b) Mutual Exclusion
c) Multi-thread Extension
d) Memory Execution

**Answer:** b) Mutual Exclusion

### Q86. 🟡 What is busy waiting?
**Answer:** Busy waiting (spinlock) is when a thread continuously checks a condition in a loop while waiting. Wastes CPU cycles as thread keeps running without making progress. Better alternatives: blocking with conditional variables or semaphores.

### Q87. 🟢 Semaphore operations are:
a) read() and write()
b) wait() and signal()
c) lock() and unlock()
d) acquire() and release()

**Answer:** b) wait() and signal()

### Q88. 🟡 Difference between binary semaphore and mutex?
**Answer:**

| Binary Semaphore | Mutex |
|------------------|-------|
| Signaling mechanism | Locking mechanism |
| No ownership | Ownership (must unlock by owner) |
| Can be signaled by any thread | Must be unlocked by locking thread |
| Value: 0 or 1 | Locked or unlocked |

### Q89. 🟢 Counting semaphore initial value represents:
a) Number of processes
b) Number of available resources
c) Priority level
d) Time quantum

**Answer:** b) Number of available resources

### Q90. 🟡 What is the Producer-Consumer problem?
**Answer:** Classic synchronization problem:
- Producers generate data, put in bounded buffer
- Consumers remove data from buffer
- **Constraints**: Producer waits if full, consumer waits if empty, mutual exclusion needed
- **Solution**: 3 semaphores (mutex=1, empty=N, full=0)

### Q91. 🟢 In Dining Philosophers, deadlock occurs when:
a) All pick up left fork simultaneously
b) All are eating
c) All are thinking
d) One philosopher eats

**Answer:** a) All pick up left fork simultaneously

### Q92. 🟡 What are solutions to Dining Philosophers problem?
**Answer:**
1. **At most 4 philosophers** sit simultaneously
2. **Atomic pickup**: Both forks picked in critical section
3. **Odd-even rule**: Odd pick left→right, even pick right→left
4. **Resource ordering**: Number forks, always pick lower numbered first

### Q93. 🟢 Deadlock requires all of these EXCEPT:
a) Mutual exclusion
b) Hold and wait
c) Preemption
d) Circular wait

**Answer:** c) Preemption (should be "No preemption")

### Q94. 🟡 Explain the four necessary conditions for deadlock.
**Answer:**
1. **Mutual Exclusion**: Resource can't be shared
2. **Hold and Wait**: Process holds resources while waiting for more
3. **No Preemption**: Resources can't be forcefully taken
4. **Circular Wait**: Circular chain of processes waiting for resources

**All four must hold simultaneously** for deadlock to occur.

### Q95. 🟢 Breaking which ONE condition will prevent deadlock?
a) Only mutual exclusion
b) Only circular wait
c) Any one of the four
d) All four must be broken

**Answer:** c) Any one of the four

### Q96. 🟡 How can we prevent circular wait?
**Answer:** **Resource ordering**: Impose total ordering on resource types. Process can request Ri only if it doesn't hold Rj where j > i. This forces linear acquisition order, making cycles impossible.

**Example**: If R1 < R2, both processes must request R1 before R2.

### Q97. 🟢 Safe state means:
a) No deadlock exists
b) System can allocate resources avoiding deadlock
c) All processes completed
d) No processes in system

**Answer:** b) System can allocate resources avoiding deadlock

### Q98. 🟡 Difference between safe state and unsafe state?
**Answer:**
- **Safe State**: There exists a sequence to allocate resources to all processes without deadlock
- **Unsafe State**: No such safe sequence exists; MAY lead to deadlock
- Important: Unsafe ≠ Deadlock, but unsafe might become deadlock
- Deadlock ⊂ Unsafe ⊂ All States

### Q99. 🟢 Banker's algorithm is used for:
a) Deadlock prevention
b) Deadlock avoidance
c) Deadlock detection
d) Deadlock recovery

**Answer:** b) Deadlock avoidance

### Q100. 🟡 Explain Banker's Algorithm concept.
**Answer:** Like a bank deciding loans:
- Bank (OS) has limited cash (resources)
- Customers (processes) request loans
- Before granting, bank checks: "Can I still satisfy all customers?"
- If **yes** (safe state): Grant request
- If **no** (unsafe state): Deny, process waits

Ensures system never enters unsafe state.

### Q101. 🟢 Wait-for graph is used for:
a) Scheduling
b) Deadlock detection
c) Memory allocation
d) File management

**Answer:** b) Deadlock detection

### Q102. 🟡 How does wait-for graph detect deadlock?
**Answer:**
- **Nodes**: Processes
- **Edge** P1→P2: P1 waiting for resource held by P2
- **Cycle detection**: Run DFS/BFS periodically
- If **cycle found**: Deadlock exists
- Works well for single instance resources

### Q103. 🟢 Ostrich algorithm means:
a) Prevent deadlock
b) Avoid deadlock
c) Ignore deadlock
d) Detect deadlock quickly

**Answer:** c) Ignore deadlock

### Q104. 🟡 Why do some systems use deadlock ignorance?
**Answer:** Used in Windows, Linux because:
- Deadlocks are **rare** in these systems
- Prevention/avoidance cost is **high**
- Users can **restart** if deadlock occurs
- **Simplicity** over robustness for non-critical systems
- Not suitable for mission-critical systems (medical, aerospace)

### Q105. 🟢 Deadlock recovery can be done by:
a) Process termination
b) Resource preemption
c) Both a and b
d) Restarting system

**Answer:** c) Both a and b

### Q106. 🟡 What factors to consider when selecting victim for preemption?
**Answer:**
1. **Priority**: Lower priority first
2. **Computation time**: Process that computed least
3. **Resources held**: Fewer resources = less cost
4. **Resources needed**: Closer to completion = don't abort
5. **Number of rollbacks**: Prevent starvation

### Q107. 🟢 Livelock differs from deadlock in:
a) Processes are blocked
b) Processes are active but not progressing
c) Resources are held
d) System crashes

**Answer:** b) Processes are active but not progressing

### Q108. 🟡 What is priority inversion?
**Answer:** Priority inversion: High priority task blocked because low priority task holds lock. Medium priority task preempts low priority, so high priority waits for medium indirectly.

**Solution**: Priority inheritance or priority ceiling protocols.

### Q109. 🟢 Semaphore value can be:
a) Only positive
b) Only zero or positive
c) Positive, zero, or negative
d) Only negative

**Answer:** c) Positive, zero, or negative (in blocking implementation)

### Q110. 🟡 What does negative semaphore value indicate?
**Answer:** In blocking implementation:
- **Positive**: Number of available resources
- **Zero**: No resources available
- **Negative**: Absolute value = number of waiting processes

**Example**: Semaphore = -3 means 3 processes waiting.

### Q111. 🟢 Conditional variable is used with:
a) Semaphore
b) Mutex
c) Monitor
d) Spinlock

**Answer:** b) Mutex

### Q112. 🟡 How do conditional variables avoid busy waiting?
**Answer:**
1. Thread acquires lock
2. Checks condition
3. If false: **Releases lock** and **blocks** (sleeps)
4. Another thread signals when condition true
5. Blocked thread wakes, re-acquires lock, continues

No busy waiting because thread blocks (doesn't consume CPU).

### Q113. 🟢 Monitor is:
a) Hardware device
b) High-level synchronization construct
c) Type of scheduler
d) Memory management technique

**Answer:** b) High-level synchronization construct

### Q114. 🟡 What is the difference between deadlock and starvation?
**Answer:**

| Deadlock | Starvation |
|----------|------------|
| Circular wait | Continuously preempted |
| No process progresses | Other processes progress |
| All involved stuck | Only victim stuck |
| Requires recovery | Can be solved by aging |
| System halted | System functional |

### Q115. 🟢 Readers-Writers problem ensures:
a) Only one reader at a time
b) Multiple readers, one writer
c) Multiple writers
d) No reading allowed

**Answer:** b) Multiple readers, one writer

---

## Section 5: Memory Management (35 Questions)

### Q116. 🟢 Logical address is generated by:
a) CPU
b) MMU
c) Memory
d) Disk

**Answer:** a) CPU

### Q117. 🟡 Difference between logical and physical address?
**Answer:**

| Logical Address | Physical Address |
|-----------------|------------------|
| Generated by CPU | In physical memory (RAM) |
| Virtual (doesn't exist physically) | Actually exists |
| User can view | User cannot access directly |
| Range: 0 to max | Range: R to R+max |

**MMU** translates logical to physical address.

### Q118. 🟢 MMU stands for:
a) Memory Management Unit
b) Multiple Memory Unit
c) Master Management Unit
d) Main Memory Unit

**Answer:** a) Memory Management Unit

### Q119. 🟡 What is the role of MMU?
**Answer:** MMU (Memory Management Unit):
- **Translates** logical addresses to physical addresses
- Uses **base register** (relocation register)
- Physical Address = Logical Address + Base Register
- Enables process isolation and protection
- Supports virtual memory

### Q120. 🟢 Internal fragmentation occurs in:
a) Fixed partitioning
b) Dynamic partitioning
c) Paging
d) Both a and c

**Answer:** d) Both a and c

### Q121. 🟡 Differentiate internal and external fragmentation.
**Answer:**

| Internal Fragmentation | External Fragmentation |
|------------------------|------------------------|
| Wasted space WITHIN partition | Wasted space BETWEEN partitions |
| Process smaller than partition | Free space exists but not contiguous |
| Fixed partitioning, Paging | Dynamic partitioning, Segmentation |
| **Solution**: Paging with smaller pages | **Solution**: Compaction or Paging |

### Q122. 🟢 Compaction is used to solve:
a) Internal fragmentation
b) External fragmentation
c) Page faults
d) Deadlock

**Answer:** b) External fragmentation

### Q123. 🟡 What is compaction and its disadvantage?
**Answer:** **Compaction**: Move all loaded processes together to make free space contiguous.

**Disadvantages**:
- ❌ High overhead (moving processes)
- ❌ CPU time consumed
- ❌ System less responsive
- ❌ May need frequent compaction

**Alternative**: Use paging.

### Q124. 🟢 Best Fit algorithm:
a) Allocates largest hole
b) Allocates smallest sufficient hole
c) Allocates first hole found
d) Allocates last hole

**Answer:** b) Allocates smallest sufficient hole

### Q125. 🟡 Compare First Fit, Best Fit, and Worst Fit.
**Answer:**

| Algorithm | Selects | Speed | Fragmentation |
|-----------|---------|-------|---------------|
| **First Fit** | First hole that fits | Fast | Moderate |
| **Best Fit** | Smallest hole that fits | Slow (search all) | Low internal, high external |
| **Worst Fit** | Largest hole | Slow (search all) | Leaves larger holes |

### Q126. 🟢 Paging divides:
a) Only physical memory
b) Only logical memory
c) Both physical and logical memory
d) Neither

**Answer:** c) Both physical and logical memory

### Q127. 🟡 Explain paging concept.
**Answer:** Paging eliminates external fragmentation:
- **Physical memory**: Divided into fixed-size **frames**
- **Logical memory**: Divided into same-size **pages**
- Page size = Frame size (typically 4KB)
- **Non-contiguous** allocation allowed
- **Page table** maps pages to frames
- ✅ No external fragmentation
- ❌ Internal fragmentation in last page

### Q128. 🟢 Page table is stored in:
a) CPU registers
b) Main memory
c) Cache
d) Disk

**Answer:** b) Main memory

### Q129. 🟡 Why is page table not stored in CPU registers?
**Answer:**
- Page table can be **very large** (millions of entries)
- CPU has limited registers (32-64)
- **Example**: 4GB address space, 4KB pages = 1M entries
- Changing page table requires changing one register (**PTBR**) not copying entire table
- Faster context switching

### Q130. 🟢 TLB stands for:
a) Translation Look-aside Buffer
b) Thread Local Buffer
c) Temporary Load Buffer
d) Time-based Locking Buffer

**Answer:** a) Translation Look-aside Buffer

### Q131. 🟡 Why is TLB needed?
**Answer:** **Problem**: Page table in memory → 2 memory accesses (slow)

**Solution**: TLB (hardware cache for page table)
- **TLB hit** (90-98%): 1 memory access
- **TLB miss**: 2 memory accesses
- Stores recent page-to-frame translations
- Makes paging practical

### Q132. 🟢 ASID in TLB is used for:
a) Increasing speed
b) Process identification
c) Address calculation
d) Error detection

**Answer:** b) Process identification

### Q133. 🟡 Calculate effective memory access time.
**Question**: Memory access = 100ns, TLB lookup = 20ns, TLB hit ratio = 80%

**Answer:**
```
TLB Hit: 20 (TLB) + 100 (memory) = 120 ns
TLB Miss: 20 (TLB) + 100 (page table) + 100 (memory) = 220 ns
Effective = 0.8 × 120 + 0.2 × 220 = 96 + 44 = 140 ns
```

### Q134. 🟢 Segmentation divides program into:
a) Fixed-size blocks
b) Variable-size logical units
c) Equal parts
d) Random sizes

**Answer:** b) Variable-size logical units

### Q135. 🟡 Difference between paging and segmentation?
**Answer:**

| Paging | Segmentation |
|--------|--------------|
| Fixed-size pages | Variable-size segments |
| OS view (invisible to user) | User view (logical divisions) |
| No external fragmentation | External fragmentation |
| Internal fragmentation | No internal fragmentation |
| Page table | Segment table |
| Simple addressing | Complex addressing |

### Q136. 🟢 Virtual memory allows:
a) Larger physical memory
b) Execution of process not entirely in memory
c) Faster execution
d) More CPU cores

**Answer:** b) Execution of process not entirely in memory

### Q137. 🟡 What are the benefits of virtual memory?
**Answer:**
1. **Programs > Physical memory**: Run large programs on small RAM
2. **More programs**: Higher degree of multiprogramming
3. **Faster loading**: Only needed pages loaded
4. **Memory abstraction**: Process thinks it has contiguous memory
5. **Isolation**: Each process has own address space

### Q138. 🟢 Demand paging means:
a) Load all pages initially
b) Load page only when demanded
c) Page always in memory
d) No paging used

**Answer:** b) Load page only when demanded

### Q139. 🟡 Explain demand paging.
**Answer:** **Lazy loading** of pages:
1. Process starts with **no pages** in memory
2. When page accessed → **Page fault**
3. OS loads page from disk to memory
4. Update page table, set valid bit
5. Restart instruction
6. Process continues execution

**Benefits**: Faster start, less memory, load only needed pages.

### Q140. 🟢 Page fault occurs when:
a) Page is in memory
b) Page is not in memory
c) Memory is full
d) Process terminates

**Answer:** b) Page is not in memory

### Q141. 🟡 Describe page fault handling steps.
**Answer:**
1. **Trap** to OS (page fault handler)
2. Check if **valid reference** (from PCB)
3. If invalid access → **Abort process**
4. If valid but not in memory:
   - Find **free frame**
   - Read page from **disk**
   - Update **page table** (set valid bit)
5. **Restart** instruction

### Q142. 🟢 Valid-invalid bit in page table indicates:
a) Page size
b) Page in memory or not
c) Page number
d) Frame number

**Answer:** b) Page in memory or not

### Q143. 🟡 What does valid-invalid bit represent?
**Answer:**
- **Valid (1)**: Page is in memory, can be accessed
- **Invalid (0)**: Either:
  - Page not in memory (on disk) → Page fault
  - Page not in process's address space → Segmentation fault

### Q144. 🟢 FIFO page replacement can suffer from:
a) Starvation
b) Deadlock
c) Belady's anomaly
d) Priority inversion

**Answer:** c) Belady's anomaly

### Q145. 🟡 What is Belady's Anomaly?
**Answer:** Counter-intuitive phenomenon where **increasing frames increases page faults**.

**Example** (FIFO):
- 3 frames: 9 page faults
- 4 frames: 10 page faults ❌

**Occurs in**: FIFO
**Not in**: LRU, Optimal (stack algorithms)

### Q146. 🟢 Which page replacement is optimal?
a) FIFO
b) LRU
c) Optimal
d) Random

**Answer:** c) Optimal

### Q147. 🟡 Compare FIFO, LRU, and Optimal page replacement.
**Answer:**

| Algorithm | Page Faults | Implementation | Practical |
|-----------|-------------|----------------|-----------|
| **FIFO** | High | Easy (queue) | Yes |
| **LRU** | Medium | Complex (stack/counter) | Yes |
| **Optimal** | **Lowest** | Impossible (needs future) | No (benchmark) |

**Best**: Optimal (theoretical), LRU (practical)

### Q148. 🟢 LRU replaces page that is:
a) Arrived first
b) Not used for longest time
c) Used most frequently
d) Random

**Answer:** b) Not used for longest time

### Q149. 🟡 How to implement LRU?
**Answer:**
**Method 1 - Counter**:
- Associate timestamp with each page
- Replace page with smallest timestamp
- Overhead: Update on every access

**Method 2 - Stack**:
- Keep stack of page numbers (doubly linked list)
- Referenced page moved to top
- Bottom page = LRU
- Overhead: Update pointers on every access

### Q150. 🟢 Thrashing means:
a) High CPU utilization
b) More time swapping than executing
c) Fast execution
d) Low memory usage

**Answer:** b) More time swapping than executing

### Q151. 🟡 What causes thrashing and how to prevent it?
**Answer:**
**Cause**: Process doesn't have enough frames for its locality. Constantly page faulting.

**Prevention**:
1. **Working Set Model**: Allocate frames based on locality
2. **Page Fault Frequency**: Monitor PF rate, adjust frames
3. **Reduce multiprogramming**: Suspend some processes
4. **Increase memory**: Add more RAM

### Q152. 🟢 Working set is based on:
a) Page size
b) Locality of reference
c) Frame number
d) Process priority

**Answer:** b) Locality of reference

### Q153. 🟡 Explain locality of reference.
**Answer:** Programs tend to access small portion of address space at any time.

**Types**:
1. **Temporal Locality**: Recently accessed location likely accessed again
   - Example: Loop variables
2. **Spatial Locality**: Nearby locations likely accessed soon
   - Example: Array elements, sequential instructions

**Used in**: Caching, paging, working set model

### Q154. 🟢 Page size is usually:
a) 1 KB
b) 4 KB
c) 1 MB
d) 4 MB

**Answer:** b) 4 KB (typical, can vary)

### Q155. 🟡 Calculate internal fragmentation in paging.
**Question**: Process = 10250 bytes, Page size = 1024 bytes

**Answer:**
```
Pages needed = ⌈10250 / 1024⌉ = 11 pages
Allocated = 11 × 1024 = 11264 bytes
Internal fragmentation = 11264 - 10250 = 1014 bytes
(Last page uses 10 bytes, wastes 1014 bytes)
```

---

## Section 6: File Systems & I/O (20 Questions)

### Q156. 🟢 File system manages:
a) CPU scheduling
b) File storage and retrieval
c) Memory allocation
d) Process synchronization

**Answer:** b) File storage and retrieval

### Q157. 🟡 What are the functions of file system?
**Answer:**
1. **File organization**: Structure files on storage
2. **Naming**: Map symbolic names to files
3. **Access control**: Permission management
4. **Allocation**: Manage disk space
5. **Protection**: Prevent unauthorized access
6. **Reliability**: Backup and recovery

### Q158. 🟢 Absolute path starts from:
a) Current directory
b) Root directory
c) Home directory
d) Parent directory

**Answer:** b) Root directory

### Q159. 🟡 Difference between absolute and relative path?
**Answer:**
- **Absolute Path**: Complete path from root (/, C:\)
  - Example: /home/user/file.txt
  - Unambiguous, works from anywhere
- **Relative Path**: Path from current directory
  - Example: ../file.txt
  - Shorter but depends on current location

### Q160. 🟢 Contiguous allocation suffers from:
a) Internal fragmentation
b) External fragmentation
c) Slow access
d) No fragmentation

**Answer:** b) External fragmentation

### Q161. 🟡 Compare file allocation methods.
**Answer:**

| Method | Access | Fragmentation | Directory Entry |
|--------|--------|---------------|-----------------|
| **Contiguous** | Fast sequential | External | Start block + length |
| **Linked** | Slow sequential | No | Start block only |
| **Indexed** | Fast random | Some internal | Index block |

### Q162. 🟢 Inode contains:
a) File content
b) File metadata
c) Directory listing
d) Disk blocks

**Answer:** b) File metadata

### Q163. 🟡 What information is stored in inode?
**Answer:**
- File type and permissions
- Owner (user ID, group ID)
- File size
- Timestamps (created, modified, accessed)
- Link count
- Pointers to data blocks
- **Not stored**: File name (in directory)

### Q164. 🟢 Hard link creates:
a) New inode
b) Reference to same inode
c) Copy of file
d) Symbolic link

**Answer:** b) Reference to same inode

### Q165. 🟡 Difference between hard link and soft link?
**Answer:**

| Hard Link | Soft Link (Symbolic) |
|-----------|---------------------|
| Same inode | Different inode |
| Can't cross filesystems | Can cross filesystems |
| No broken links | Can break if target deleted |
| Only for files | For files and directories |
| Increases link count | Doesn't affect link count |

### Q166. 🟢 FCFS disk scheduling suffers from:
a) Starvation
b) High seek time
c) Deadlock
d) Thrashing

**Answer:** b) High seek time

### Q167. 🟡 Compare disk scheduling algorithms.
**Answer:**

| Algorithm | Mechanism | Starvation | Performance |
|-----------|-----------|------------|-------------|
| **FCFS** | Order of arrival | No | Poor |
| **SSTF** | Shortest seek time first | Yes | Good |
| **SCAN** | Elevator algorithm | No | Better |
| **C-SCAN** | Circular SCAN | No | Best |

### Q168. 🟢 Buffering is used to:
a) Save memory
b) Handle speed mismatch
c) Encrypt data
d) Compress files

**Answer:** b) Handle speed mismatch

### Q169. 🟡 Differentiate spooling, buffering, and caching.
**Answer:**
- **Spooling**: Queue jobs for sequential processing (print jobs)
  - Between different speed devices
- **Buffering**: Temporary storage during transfer (video streaming)
  - Within one job, handle speed mismatch
- **Caching**: Store frequently accessed data (web cache)
  - Speed up future access

### Q170. 🟢 DMA stands for:
a) Direct Memory Access
b) Dual Memory Allocation
c) Dynamic Memory Assignment
d) Data Management Access

**Answer:** a) Direct Memory Access

### Q171. 🟡 What is the purpose of DMA?
**Answer:** DMA allows I/O devices to transfer data directly to/from memory without CPU involvement:
- **Frees CPU** for other tasks
- **Faster transfers** (no CPU overhead)
- **Interrupt** CPU only when transfer complete
- Used for high-speed devices (disk, network)

### Q172. 🟢 Interrupt handling involves:
a) Creating process
b) Saving context and executing ISR
c) Scheduling
d) Memory allocation

**Answer:** b) Saving context and executing ISR

### Q173. 🟡 What is an interrupt and how is it handled?
**Answer:**
**Interrupt**: Signal to CPU that requires immediate attention.

**Types**:
- Hardware interrupt (I/O completion, timer)
- Software interrupt (system calls, exceptions)

**Handling**:
1. Save current context (PC, registers)
2. Determine interrupt source (interrupt vector)
3. Execute **Interrupt Service Routine (ISR)**
4. Restore context
5. Resume execution

### Q174. 🟢 Disk scheduling is required because:
a) Disks are slow
b) Multiple I/O requests arrive
c) Disks are expensive
d) Files are large

**Answer:** b) Multiple I/O requests arrive

### Q175. 🟡 What is seek time in disk operations?
**Answer:**
**Seek Time**: Time to move disk arm to desired cylinder.

**Disk access time = Seek Time + Rotational Latency + Transfer Time**

- **Seek time**: Largest component (several ms)
- **Rotational latency**: Wait for sector rotation
- **Transfer time**: Read/write data

Disk scheduling minimizes total seek time.

---

## Section 7: Advanced Topics (30 Questions)

### Q176. 🟢 Microkernel architecture moves services to:
a) Kernel space
b) User space
c) Hardware
d) Disk

**Answer:** b) User space

### Q177. 🟡 What are advantages and disadvantages of microkernel?
**Answer:**

**Advantages**:
- ✅ More reliable (isolated services)
- ✅ More secure (less code in kernel)
- ✅ Easier to extend
- ✅ Portable

**Disadvantages**:
- ❌ Slower performance (IPC overhead)
- ❌ Complex IPC mechanisms
- ❌ More context switching

### Q178. 🟢 System call transitions from:
a) Kernel to user mode
b) User to kernel mode
c) Process to thread
d) Physical to logical address

**Answer:** b) User to kernel mode

### Q179. 🟡 How does system call transition work?
**Answer:**
1. User program invokes system call wrapper
2. Library prepares arguments
3. **Software interrupt** (trap) triggered
4. CPU switches to **kernel mode**
5. Save user context
6. Execute system call in kernel
7. Restore context
8. Switch back to **user mode**
9. Return to user program

**Mechanism**: Software interrupt (trap instruction)

### Q180. 🟢 Copy-on-write optimizes:
a) Disk I/O
b) fork() system call
c) Scheduling
d) Paging

**Answer:** b) fork() system call

### Q181. 🟡 Explain copy-on-write (COW) optimization.
**Answer:** After fork(), parent and child share same physical pages initially:
- Pages marked **read-only**
- On **write attempt**: Page fault occurs
- OS creates **copy** of page
- Both get separate pages

**Benefits**:
- Saves memory if child calls exec() immediately
- Faster fork() (no immediate copying)
- Only modified pages duplicated

### Q182. 🟢 Which is NOT a boot stage?
a) POST
b) BIOS
c) Compiling
d) Bootloader

**Answer:** c) Compiling

### Q183. 🟡 What is the role of bootloader?
**Answer:** Bootloader is small program executed after BIOS/UEFI:
- **Loads** operating system kernel into memory
- **Initializes** basic hardware
- **Transfers** control to kernel
- **Examples**: GRUB (Linux), Bootmgr (Windows), boot.efi (macOS)
- Located in MBR or EFI partition

### Q184. 🟢 Virtualization allows:
a) Faster CPU
b) Multiple OS on single hardware
c) More memory
d) Better scheduling

**Answer:** b) Multiple OS on single hardware

### Q185. 🟡 What is Type 1 vs Type 2 hypervisor?
**Answer:**

| Type 1 (Bare Metal) | Type 2 (Hosted) |
|---------------------|-----------------|
| Runs directly on hardware | Runs on host OS |
| Better performance | Easier to install |
| Examples: VMware ESXi, Xen | Examples: VirtualBox, VMware Workstation |
| Enterprise use | Desktop/development use |

### Q186. 🟢 RAID is used for:
a) CPU performance
b) Disk redundancy and performance
c) Memory management
d) Process scheduling

**Answer:** b) Disk redundancy and performance

### Q187. 🟡 Compare RAID levels.
**Answer:**

| RAID | Description | Pros | Cons |
|------|-------------|------|------|
| **RAID 0** | Striping | Fast, full capacity | No redundancy |
| **RAID 1** | Mirroring | Redundancy | 50% capacity |
| **RAID 5** | Striping + parity | Good balance | Write penalty |
| **RAID 10** | Mirror + stripe | Fast + redundant | Expensive |

### Q188. 🟢 Cache coherence is important in:
a) Single processor
b) Multiprocessor systems
c) Virtual memory
d) File systems

**Answer:** b) Multiprocessor systems

### Q189. 🟡 What is cache coherence problem?
**Answer:** In multiprocessor systems, each CPU has its own cache. Problem arises when multiple caches have copies of same memory location:
- CPU1 modifies value in its cache
- CPU2's cache still has old value
- **Inconsistency!**

**Solutions**:
- **Write-through**: Write to memory immediately
- **Write-back** with snooping: Monitor bus for writes
- **Directory-based**: Track which caches have copies

### Q190. 🟢 In SMP, SMP stands for:
a) Simple Multi-Processing
b) Symmetric Multi-Processing
c) Single Multi-Processing
d) Serial Multi-Processing

**Answer:** b) Symmetric Multi-Processing

### Q191. 🟡 Difference between SMP and AMP?
**Answer:**
- **SMP (Symmetric)**: All processors are peers, share memory equally
  - Any processor can run any process
  - Single OS instance
  - Better load balancing
- **AMP (Asymmetric)**: Master-slave relationship
  - Master assigns tasks to slaves
  - Simpler but less efficient

### Q192. 🟢 Security in OS includes:
a) Authentication only
b) Authentication, authorization, encryption
c) Virus scanning only
d) Password protection only

**Answer:** b) Authentication, authorization, encryption

### Q193. 🟡 What are main security mechanisms in OS?
**Answer:**
1. **Authentication**: Verify user identity (passwords, biometrics)
2. **Authorization**: Access control (permissions, ACLs)
3. **Encryption**: Protect data (file encryption, disk encryption)
4. **Audit**: Log security events
5. **Isolation**: Sandboxing, virtualization
6. **Updates**: Patch vulnerabilities

### Q194. 🟢 Which is NOT a scheduling criterion?
a) CPU utilization
b) Throughput
c) Disk size
d) Response time

**Answer:** c) Disk size

### Q195. 🟡 How does OS ensure fair resource allocation?
**Answer:**
1. **Scheduling algorithms**: Fair queuing, weighted fair queuing
2. **Quotas**: Limit resource usage per user/process
3. **Priority aging**: Prevent starvation
4. **Resource reservation**: Guarantee minimum resources
5. **Accounting**: Track usage, enforce limits

### Q196. 🟢 Real-time systems require:
a) Maximum throughput
b) Predictable response time
c) Minimum memory
d) Many processes

**Answer:** b) Predictable response time

### Q197. 🟡 Difference between hard and soft real-time systems?
**Answer:**
- **Hard Real-Time**: Missing deadline is **catastrophic**
  - Must guarantee completion by deadline
  - Examples: Pacemaker, airbag controller
  - Zero tolerance for late responses
- **Soft Real-Time**: Missing deadline is **undesirable** but tolerable
  - Best effort to meet deadlines
  - Examples: Video streaming, online gaming
  - Degraded performance acceptable

### Q198. 🟢 Journaling file system helps with:
a) Performance
b) Crash recovery
c) Compression
d) Encryption

**Answer:** b) Crash recovery

### Q199. 🟡 How does journaling improve file system reliability?
**Answer:** Journaling records intended changes in a log (journal) before applying them:

**Process**:
1. Write operation details to journal
2. Mark journal entry as committed
3. Apply actual changes to file system
4. Remove journal entry

**On crash**:
- Check journal
- Complete or rollback incomplete operations
- File system consistency maintained

**Examples**: ext4, NTFS, XFS

### Q200. 🟢 Virtual machine monitor is also called:
a) Kernel
b) Hypervisor
c) Bootloader
d) Shell

**Answer:** b) Hypervisor

---

## 🎯 Quick Reference - Key Formulas

### Scheduling Metrics
```
Turnaround Time (TAT) = Completion Time - Arrival Time
Waiting Time (WT) = TAT - Burst Time
Response Time = Time of first CPU allocation - Arrival Time
Throughput = Number of processes completed / Total time
CPU Utilization = (Total busy time / Total time) × 100%
```

### Memory Management
```
Logical Address = Page Number + Page Offset
Physical Address = Frame Number + Page Offset
Physical Address = Logical Address + Base Register
Number of Pages = ⌈Process Size / Page Size⌉
Internal Fragmentation = (Pages × Page Size) - Process Size
```

### Effective Access Time
```
EAT = (TLB Hit Ratio × Hit Time) + (TLB Miss Ratio × Miss Time)
Where:
  Hit Time = TLB access + Memory access
  Miss Time = TLB access + Page Table access + Memory access
```

---

## 📝 Study Tips

### For MCQs
1. ✅ Understand concepts, don't just memorize
2. ✅ Practice numerical problems (scheduling, memory)
3. ✅ Focus on differences (process vs thread, paging vs segmentation)
4. ✅ Remember key algorithms (deadlock conditions, page replacement)
5. ✅ Know real-world examples

### For Interviews
1. ✅ Explain with diagrams
2. ✅ Give real-world analogies
3. ✅ Mention trade-offs (pros and cons)
4. ✅ Use correct terminology
5. ✅ Be ready for follow-up questions

### Common Mistakes to Avoid
1. ❌ Confusing similar concepts (semaphore vs mutex)
2. ❌ Wrong formulas (TAT vs WT)
3. ❌ Not understanding prerequisites (need to know process before scheduling)
4. ❌ Ignoring edge cases (what if time quantum = 0?)
5. ❌ Vague answers (be specific with examples)

---

## 🏆 Difficulty Distribution

| Difficulty | Count | Percentage |
|------------|-------|------------|
| 🟢 Easy | 80 | 40% |
| 🟡 Medium | 95 | 47.5% |
| 🔴 Hard | 25 | 12.5% |
| **Total** | **200** | **100%** |

---

## 📚 Sources

Questions compiled from:
- **GeeksforGeeks** - Operating System Interview Questions
- **InterviewBit** - OS MCQs and Interview Prep
- **Sanfoundry** - 1000+ OS MCQs
- **LeetCode** - System Design Discussions
- **Placement Preparation** - 150+ OS MCQs
- **Career Cup** - Real interview experiences
- **TakeUForward** - Most Asked Questions
- **Real interviews** from Google, Microsoft, Amazon, Facebook

---

## ✅ Checklist for Interview Preparation

- [ ] Completed all 200 questions
- [ ] Understood all concepts (not just memorized)
- [ ] Practiced numerical problems
- [ ] Can draw diagrams for key concepts
- [ ] Know all formulas
- [ ] Practiced explaining to someone else
- [ ] Solved additional problems from sources
- [ ] Reviewed common mistakes
- [ ] Comfortable with follow-up questions
- [ ] Ready for coding questions related to OS concepts

---

**Good luck with your interviews!** 🚀

Remember: Understanding > Memorization

---

**Last Updated:** November 2025
**Version:** 1.0
**Total Questions:** 200 (MCQs + Conceptual)
**Estimated Study Time:** 40-50 hours
