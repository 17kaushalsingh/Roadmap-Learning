# Operating Systems Fast Revision ⚙️

## Why OS is Medium Priority 🎯

**Fundamental concepts used in coding. Interviewers try to go deep into deadlocks, semaphores, and memory management often because these are things that are also used in codes. The clarity of operating systems really matters.**

### **Key Interview Focus**:
- **Understanding of system-level programming**
- **Process and thread management**
- **Synchronization and concurrency**
- **Memory management concepts**
- **Real-world application of OS concepts**

---

## Core OS Concepts 📋

### **1. Process vs Thread** 🔄

#### **Process**:
```
Definition: A program in execution with its own memory space
Characteristics:
- Independent execution unit
- Has its own memory space
- Heavyweight context switching
- Inter-process communication required
```

#### **Thread**:
```
Definition: A lightweight execution unit within a process
Characteristics:
- Shares memory with other threads in same process
- Lightweight context switching
- Can communicate directly (shared memory)
- Part of a process
```

#### **Key Differences**:
| **Aspect** | **Process** | **Thread** |
|------------|-------------|------------|
| **Memory** | Separate memory space | Shared memory space |
| **Communication** | IPC mechanisms | Direct sharing |
| **Context Switch** | Heavyweight | Lightweight |
| **Creation** | Expensive | Cheap |
| **Resource Usage** | High | Low |

#### **Interview Code Example**:
```java
// Process vs Thread in Java
public class ProcessThreadDemo {
    public static void main(String[] args) {
        // Creating a new process (using ProcessBuilder)
        ProcessBuilder pb = new ProcessBuilder("notepad.exe");

        // Creating threads
        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Thread 1: " + i);
                try { Thread.sleep(1000); } catch (InterruptedException e) {}
            }
        });

        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Thread 2: " + i);
                try { Thread.sleep(1000); } catch (InterruptedException e) {}
            }
        });

        thread1.start();
        thread2.start();
    }
}
```

#### **Common Interview Questions**:
- **Difference between process and thread?**
- **Why thread context switching is faster?**
- **When to use processes vs threads?**
- **Can a process exist without threads?**

### **2. Process States** 🔄

#### **State Diagram**:
```
    [New] → [Ready] → [Running] → [Terminated]
             ↑         ↓         ↓
             │    [Waiting] ← [I/O or Event]
             └─────────────────┘
```

#### **State Descriptions**:

| **State** | **Description** | **Example** |
|------------|-----------------|-------------|
| **New** | Process being created | `Process p = new Process()` |
| **Ready** | Waiting for CPU | Process waiting in queue |
| **Running** | Currently executing | CPU executing instructions |
| **Waiting/Blocked** | Waiting for I/O or event | `file.read()` waiting |
| **Terminated** | Finished execution | Process completed |

#### **Interview Questions**:
- **Explain process life cycle?**
- **When does a process move from running to waiting?**
- **Difference between ready and waiting state?**

### **3. CPU Scheduling Algorithms** ⏰

#### **1. First Come First Serve (FCFS)**
```java
// FCFS Example
class FCFSScheduler {
    public void schedule(List<Process> processes) {
        int currentTime = 0;
        for (Process process : processes) {
            process.waitingTime = currentTime - process.arrivalTime;
            currentTime += process.burstTime;
            process.turnaroundTime = currentTime - process.arrivalTime;
        }
    }
}

// Problems: Convoy effect
// Average waiting time can be high
```

#### **2. Shortest Job First (SJF)**
```java
class SJFScheduler {
    public void schedule(List<Process> processes) {
        PriorityQueue<Process> queue = new PriorityQueue<>(
            Comparator.comparingInt(p -> p.burstTime)
        );

        int currentTime = 0;
        while (!processes.isEmpty() || !queue.isEmpty()) {
            // Add processes that have arrived
            for (Process p : processes) {
                if (p.arrivalTime <= currentTime) {
                    queue.add(p);
                }
            }

            if (!queue.isEmpty()) {
                Process current = queue.poll();
                current.waitingTime = currentTime - current.arrivalTime;
                currentTime += current.burstTime;
                current.turnaroundTime = currentTime - current.arrivalTime;
            } else {
                currentTime++;
            }
        }
    }
}

// Advantage: Minimum average waiting time
// Problem: Starvation for long processes
```

#### **3. Round Robin (RR)**
```java
class RoundRobinScheduler {
    private int timeQuantum;

    public void schedule(List<Process> processes, int timeQuantum) {
        this.timeQuantum = timeQuantum;
        Queue<Process> queue = new LinkedList<>();
        int currentTime = 0;

        while (!processes.isEmpty() || !queue.isEmpty()) {
            // Add newly arrived processes
            for (Process p : processes) {
                if (p.arrivalTime <= currentTime) {
                    queue.add(p);
                }
            }

            if (!queue.isEmpty()) {
                Process current = queue.poll();

                if (current.remainingTime > timeQuantum) {
                    current.remainingTime -= timeQuantum;
                    currentTime += timeQuantum;
                    queue.add(current); // Add back to queue
                } else {
                    currentTime += current.remainingTime;
                    current.remainingTime = 0;
                    current.turnaroundTime = currentTime - current.arrivalTime;
                }
            } else {
                currentTime++;
            }
        }
    }
}

// Advantage: Fair scheduling, good for time-sharing
// Problem: Context switching overhead
```

#### **Scheduling Comparison**:

| **Algorithm** | **Average Waiting Time** | **Response Time** | **Starvation** | **Best For** |
|---------------|-------------------------|-------------------|---------------|--------------|
| **FCFS** | High | Variable | No | Batch systems |
| **SJF** | Low | Variable | Yes | Batch systems |
| **Round Robin** | Medium | Consistent | No | Time-sharing |
| **Priority** | Variable | Variable | Yes | Real-time |

---

## Synchronization & Concurrency 🔒

### **1. Race Conditions** 🏃

#### **Definition**:
**When multiple threads access shared data concurrently, and the final result depends on the order of thread execution.**

#### **Example Problem**:
```java
class BankAccount {
    private int balance = 1000;

    // Problematic method - race condition
    public void withdraw(int amount) {
        if (balance >= amount) {
            // Context switch can happen here!
            Thread.sleep(100); // Simulate delay
            balance -= amount;
            System.out.println("Withdrawn: " + amount + ", Balance: " + balance);
        } else {
            System.out.println("Insufficient funds");
        }
    }

    public int getBalance() {
        return balance;
    }
}

// Race condition demonstration
public class RaceConditionDemo {
    public static void main(String[] args) {
        BankAccount account = new BankAccount();

        Thread t1 = new Thread(() -> account.withdraw(500));
        Thread t2 = new Thread(() -> account.withdraw(700));

        t1.start();
        t2.start();

        // Both threads see balance = 1000
        // Both pass the check, but final balance becomes -200!
    }
}
```

### **2. Synchronization Mechanisms** 🔐

#### **Mutex (Mutual Exclusion)**
```java
class BankAccount {
    private int balance = 1000;
    private final Object lock = new Object(); // Mutex object

    public void withdraw(int amount) {
        synchronized (lock) { // Acquire lock
            if (balance >= amount) {
                balance -= amount;
                System.out.println("Withdrawn: " + amount + ", Balance: " + balance);
            } else {
                System.out.println("Insufficient funds");
            }
        } // Release lock
    }
}
```

#### **Semaphore** 🚦
```java
import java.util.concurrent.Semaphore;

class ResourcePool {
    private final Semaphore semaphore;
    private final List<Resource> resources;

    public ResourcePool(int capacity) {
        this.semaphore = new Semaphore(capacity);
        this.resources = new ArrayList<>();
        for (int i = 0; i < capacity; i++) {
            resources.add(new Resource(i));
        }
    }

    public Resource acquireResource() throws InterruptedException {
        semaphore.acquire(); // Acquire permit
        synchronized (resources) {
            Resource resource = resources.remove(0);
            System.out.println("Resource acquired: " + resource.getId());
            return resource;
        }
    }

    public void releaseResource(Resource resource) {
        synchronized (resources) {
            resources.add(resource);
            System.out.println("Resource released: " + resource.getId());
        }
        semaphore.release(); // Release permit
    }
}

// Semaphore controls access to limited resources
```

#### **Monitor** 📊
```java
class BoundedBuffer {
    private final int[] buffer;
    private int size = 0;
    private int in = 0, out = 0;
    private final int capacity;

    public BoundedBuffer(int capacity) {
        this.capacity = capacity;
        this.buffer = new int[capacity];
    }

    // Monitor methods (implicitly synchronized)
    public synchronized void produce(int item) throws InterruptedException {
        while (size == capacity) {
            wait(); // Wait until space available
        }
        buffer[in] = item;
        in = (in + 1) % capacity;
        size++;
        System.out.println("Produced: " + item);
        notifyAll(); // Signal waiting consumers
    }

    public synchronized int consume() throws InterruptedException {
        while (size == 0) {
            wait(); // Wait until item available
        }
        int item = buffer[out];
        out = (out + 1) % capacity;
        size--;
        System.out.println("Consumed: " + item);
        notifyAll(); // Signal waiting producers
        return item;
    }
}
```

### **3. Producer-Consumer Problem** 🏭

#### **Classic Implementation**:
```java
class ProducerConsumerExample {
    private static final int BUFFER_SIZE = 5;
    private static final BoundedBuffer buffer = new BoundedBuffer(BUFFER_SIZE);

    static class Producer implements Runnable {
        @Override
        public void run() {
            try {
                for (int i = 0; i < 10; i++) {
                    buffer.produce(i);
                    Thread.sleep(1000); // Production time
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }

    static class Consumer implements Runnable {
        @Override
        public void run() {
            try {
                for (int i = 0; i < 10; i++) {
                    buffer.consume();
                    Thread.sleep(1500); // Consumption time
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }

    public static void main(String[] args) {
        Thread producer = new Thread(new Producer());
        Thread consumer = new Thread(new Consumer());

        producer.start();
        consumer.start();
    }
}

// Output shows proper synchronization
// No race conditions, no buffer overflow/underflow
```

---

## Deadlocks & Prevention 💀

### **1. Deadlock Conditions** ⚠️

#### **Four Necessary Conditions** (Coffman Conditions):

1. **Mutual Exclusion**: Resources cannot be shared
2. **Hold and Wait**: Process holds resources while waiting for others
3. **No Preemption**: Resources cannot be forcibly taken
4. **Circular Wait**: Circular chain of processes waiting for each other

#### **Deadlock Example**:
```java
class DeadlockDemo {
    private static final Object lock1 = new Object();
    private static final Object lock2 = new Object();

    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("Thread 1: Holding lock 1...");
                try { Thread.sleep(100); } catch (InterruptedException e) {}

                System.out.println("Thread 1: Waiting for lock 2...");
                synchronized (lock2) {
                    System.out.println("Thread 1: Acquired lock 1 and lock 2");
                }
            }
        });

        Thread t2 = new Thread(() -> {
            synchronized (lock2) {
                System.out.println("Thread 2: Holding lock 2...");
                try { Thread.sleep(100); } catch (InterruptedException e) {}

                System.out.println("Thread 2: Waiting for lock 1...");
                synchronized (lock1) {
                    System.out.println("Thread 2: Acquired lock 2 and lock 1");
                }
            }
        });

        t1.start();
        t2.start();
        // Deadlock! Both threads waiting forever
    }
}
```

### **2. Deadlock Prevention** 🛡️

#### **Strategy 1: Eliminate Mutual Exclusion**
```java
// Make resources sharable when possible
class SharableResource {
    private final AtomicInteger counter = new AtomicInteger(0);

    public void increment() {
        counter.incrementAndGet();
    }

    public int getValue() {
        return counter.get();
    }
}
```

#### **Strategy 2: Eliminate Hold and Wait**
```java
class NoHoldAndWait {
    public void transfer(Account from, Account to, int amount) {
        // Acquire all resources at once
        synchronized (from) {
            synchronized (to) {
                if (from.getBalance() >= amount) {
                    from.withdraw(amount);
                    to.deposit(amount);
                }
            }
        }
        // Release all resources together
    }
}
```

#### **Strategy 3: Allow Preemption**
```java
class PreemptiveResource {
    private final Map<Thread, Resource> allocatedResources = new ConcurrentHashMap<>();

    public synchronized boolean acquire(Thread thread, Resource resource) {
        if (resource.isAvailable()) {
            resource.allocate(thread);
            allocatedResources.put(thread, resource);
            return true;
        } else {
            // Preempt from lower priority thread
            Thread owner = resource.getOwner();
            if (getPriority(thread) > getPriority(owner)) {
                resource.release();
                resource.allocate(thread);
                allocatedResources.put(thread, resource);
                return true;
            }
        }
        return false;
    }
}
```

#### **Strategy 4: Eliminate Circular Wait**
```java
class OrderedLocking {
    // Define a total ordering of resources
    private static final int RESOURCE_ORDER_1 = 1;
    private static final int RESOURCE_ORDER_2 = 2;

    private final Object lock1 = new Object();
    private final Object lock2 = new Object();

    public void method1() {
        // Always acquire locks in the same order
        synchronized (lock1) {  // Lower order first
            synchronized (lock2) {  // Higher order second
                // Critical section
            }
        }
    }

    public void method2() {
        // Same order prevents circular wait
        synchronized (lock1) {
            synchronized (lock2) {
                // Critical section
            }
        }
    }
}
```

### **3. Deadlock Detection** 🔍

#### **Banker's Algorithm Example**:
```java
class BankersAlgorithm {
    private final int[][] maximum; // Maximum demand of each process
    private final int[][] allocation; // Currently allocated resources
    private final int[][] need; // Remaining need of each process
    private final int[] available; // Available resources

    public boolean isSafeState() {
        int[] work = Arrays.copyOf(available, available.length);
        boolean[] finish = new boolean[need.length];
        int[] safeSequence = new int[need.length];
        int count = 0;

        while (count < need.length) {
            boolean found = false;
            for (int i = 0; i < need.length; i++) {
                if (!finish[i] && canFulfillNeed(i, work)) {
                    // Process can get its needed resources
                    for (int j = 0; j < work.length; j++) {
                        work[j] += allocation[i][j];
                    }
                    finish[i] = true;
                    safeSequence[count++] = i;
                    found = true;
                }
            }
            if (!found) {
                return false; // System in unsafe state
            }
        }
        return true; // System in safe state
    }

    private boolean canFulfillNeed(int process, int[] work) {
        for (int i = 0; i < work.length; i++) {
            if (need[process][i] > work[i]) {
                return false;
            }
        }
        return true;
    }
}
```

---

## Memory Management 🧠

### **1. Paging** 📄

#### **Concept**:
**Dividing physical memory into fixed-size blocks called frames and logical memory into blocks of same size called pages.**

#### **Page Table Structure**:
```java
class PageTable {
    private class PageTableEntry {
        int frameNumber;
        boolean validBit;     // Page is in memory
        boolean dirtyBit;     // Page was modified
        int protectionBits;   // Read/write/execute permissions
    }

    private PageTableEntry[] pageTable;
    private int pageSize;

    public int getPhysicalAddress(int logicalAddress) {
        int pageNumber = logicalAddress / pageSize;
        int offset = logicalAddress % pageSize;

        if (pageTable[pageNumber].validBit) {
            int frameNumber = pageTable[pageNumber].frameNumber;
            return frameNumber * pageSize + offset;
        } else {
            throw new PageFaultException("Page not in memory");
        }
    }
}
```

#### **TLB (Translation Lookaside Buffer)**:
```java
class TLB {
    private class TLBEntry {
        int pageNumber;
        int frameNumber;
        boolean valid;
    }

    private TLBEntry[] tlbCache;
    private int tlbSize;

    public Integer lookup(int pageNumber) {
        // Fast lookup in hardware cache
        for (TLBEntry entry : tlbCache) {
            if (entry.valid && entry.pageNumber == pageNumber) {
                return entry.frameNumber;
            }
        }
        return null; // TLB miss
    }

    public void update(int pageNumber, int frameNumber) {
        // Update TLB with new translation
        int index = pageNumber % tlbSize;
        tlbCache[index] = new TLBEntry();
        tlbCache[index].pageNumber = pageNumber;
        tlbCache[index].frameNumber = frameNumber;
        tlbCache[index].valid = true;
    }
}
```

### **2. Segmentation** 📊

#### **Concept**:
**Dividing memory into variable-size segments based on logical divisions of the program (code, data, stack, etc.).**

#### **Segment Table Structure**:
```java
class SegmentTable {
    private class SegmentTableEntry {
        int base;      // Starting physical address
        int limit;     // Length of segment
        boolean valid; // Segment is in memory
    }

    private Map<String, SegmentTableEntry> segmentTable;

    public int getPhysicalAddress(String segmentName, int offset) {
        SegmentTableEntry entry = segmentTable.get(segmentName);
        if (entry != null && entry.valid && offset < entry.limit) {
            return entry.base + offset;
        } else {
            throw new SegmentationFaultException("Invalid segment access");
        }
    }
}
```

### **3. Virtual Memory** 💾

#### **Concept**:
**Technique that allows execution of processes that may not be completely in memory. Parts of the process can be swapped in/out as needed.**

#### **Page Replacement Algorithms**:

##### **FIFO (First-In, First-Out)**
```java
class FIFOPageReplacement {
    private Queue<Integer> pageQueue;
    private Set<Integer> pagesInMemory;
    private int capacity;

    public FIFOPageReplacement(int capacity) {
        this.capacity = capacity;
        this.pageQueue = new LinkedList<>();
        this.pagesInMemory = new HashSet<>();
    }

    public int accessPage(int pageNumber) {
        if (pagesInMemory.contains(pageNumber)) {
            return 0; // Page hit
        }

        if (pagesInMemory.size() == capacity) {
            // Page replacement needed
            int victimPage = pageQueue.poll();
            pagesInMemory.remove(victimPage);
            System.out.println("Page fault: Replacing page " + victimPage + " with " + pageNumber);
        } else {
            System.out.println("Page fault: Loading page " + pageNumber);
        }

        pageQueue.offer(pageNumber);
        pagesInMemory.add(pageNumber);
        return 1; // Page fault
    }
}
```

##### **LRU (Least Recently Used)**
```java
class LRUPageReplacement {
    private LinkedHashMap<Integer, Integer> pageCache;
    private int capacity;

    public LRUPageReplacement(int capacity) {
        this.capacity = capacity;
        this.pageCache = new LinkedHashMap<Integer, Integer>(capacity, 0.75f, true) {
            @Override
            protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
                return size() > capacity;
            }
        };
    }

    public int accessPage(int pageNumber) {
        if (pageCache.containsKey(pageNumber)) {
            pageCache.get(pageNumber); // Access to update LRU order
            return 0; // Page hit
        } else {
            pageCache.put(pageNumber, 1);
            System.out.println("Page fault: Loading page " + pageNumber);
            return 1; // Page fault
        }
    }
}
```

##### **Optimal Page Replacement**
```java
class OptimalPageReplacement {
    public int getVictimPage(List<Integer> pageReferences, Set<Integer> currentPages, int currentIndex) {
        int victimPage = -1;
        int farthestUse = currentIndex;

        for (int page : currentPages) {
            int nextUse = findNextUse(pageReferences, page, currentIndex);
            if (nextUse == -1) { // Page will never be used again
                return page;
            }
            if (nextUse > farthestUse) {
                farthestUse = nextUse;
                victimPage = page;
            }
        }

        return victimPage;
    }

    private int findNextUse(List<Integer> pageReferences, int page, int currentIndex) {
        for (int i = currentIndex + 1; i < pageReferences.size(); i++) {
            if (pageReferences.get(i) == page) {
                return i;
            }
        }
        return -1; // Page never used again
    }
}
```

---

## Common Interview Questions & Solutions 💼

### **Question 1: Process vs Thread Differences** 🔍

**Answer Structure**:
1. **Memory**: Separate vs Shared
2. **Communication**: IPC vs Direct sharing
3. **Creation**: Expensive vs Cheap
4. **Context Switch**: Heavyweight vs Lightweight
5. **Resource Usage**: High vs Low

**Follow-up**: "When would you choose processes over threads?"
- **Isolation needed** (security, stability)
- **Independent memory requirements**
- **No need for shared data communication**

### **Question 2: Implement Producer-Consumer** 🏭

**Key Points to Cover**:
- **Synchronization mechanism** (synchronized, semaphore, or monitor)
- **Buffer management** (bounded buffer)
- **Wait/notify pattern**
- **Thread safety**
- **Error handling**

### **Question 3: Deadlock Prevention Strategies** 💀

**Four Strategies**:
1. **Eliminate Mutual Exclusion** (make resources sharable)
2. **Eliminate Hold and Wait** (acquire all resources atomically)
3. **Allow Preemption** (take resources from lower priority processes)
4. **Eliminate Circular Wait** (impose ordering on resource acquisition)

### **Question 4: Virtual Memory vs Physical Memory** 💾

**Key Differences**:
- **Virtual**: Logical address space, larger than physical
- **Physical**: Actual RAM, limited size
- **Paging**: Maps virtual to physical
- **Benefits**: Larger address space, memory protection, efficient memory usage

### **Question 5: Page Replacement Algorithm Comparison** 📊

| **Algorithm** | **Belady's Anomaly** | **Implementation** | **Performance** |
|---------------|---------------------|-------------------|-----------------|
| **FIFO** | Yes | Simple | Poor |
| **LRU** | No | Complex | Good |
| **Optimal** | No | Impossible (theoretical) | Best |
| **Clock** | No | Moderate | Good |

---

## OS Cheat Sheet 📝

### **Quick Reference Commands**:

#### **Process Commands**:
```bash
# List processes
ps aux
top
htop

# Kill process
kill -9 <PID>

# Background process
command &
jobs
fg %1
bg %1
```

#### **Memory Commands**:
```bash
# Memory usage
free -h
vmstat

# Page cache
cat /proc/meminfo
```

#### **File System**:
```bash
# Disk usage
df -h
du -sh

# Inode information
ls -i
stat file.txt
```

### **Important Formulas**:

#### **CPU Utilization**:
```
CPU Utilization = 1 - (Idle Time / Total Time)
```

#### **Throughput**:
```
Throughput = Number of Processes / Total Time
```

#### **Average Waiting Time**:
```
Average Waiting Time = Sum of Waiting Times / Number of Processes
```

#### **Page Fault Rate**:
```
Page Fault Rate = Number of Page Faults / Total Memory Accesses
```

---

## Interview Preparation Checklist ✅

### **Must-Know Concepts**:
- [ ] **Process vs Thread differences**
- [ ] **Process states and transitions**
- [ ] **CPU scheduling algorithms** (FCFS, SJF, Round Robin)
- [ ] **Race conditions and critical sections**
- [ ] **Synchronization mechanisms** (mutex, semaphore, monitor)
- [ ] **Producer-consumer problem**
- [ ] **Deadlock conditions and prevention**
- [ ] **Memory management** (paging, segmentation, virtual memory)
- [ ] **Page replacement algorithms** (FIFO, LRU, Optimal)

### **Common Problems to Implement**:
- [ ] **Producer-consumer with semaphore**
- [ ] **Reader-writer problem**
- [ ] **Dining philosophers problem**
- [ ] **Banker's algorithm**
- [ ] **Page replacement simulation**

### **System Programming Topics**:
- [ ] **Inter-process communication** (pipes, shared memory, sockets)
- [ ] **File system operations**
- [ ] **Memory allocation algorithms**
- [ ] **CPU scheduling simulation**

---

## Key Takeaways 💡

1. **Conceptual Understanding**: Focus on why things work the way they do
2. **Practical Application**: Connect OS concepts to real programming scenarios
3. **Problem-Solving**: Practice classic synchronization problems
4. **Algorithm Knowledge**: Understand scheduling and memory management algorithms
5. **Implementation Skills**: Be ready to write code for OS concepts
6. **System Thinking**: Understand how different OS components interact
7. **Performance Awareness**: Know trade-offs between different approaches
8. **Real-World Examples**: Connect theory to practical applications

**Remember**: OS concepts test your understanding of how computers work at a fundamental level. Focus on practical applications and real-world scenarios! 🖥️

---

**Next Steps**: Practice implementing these OS concepts and move on to DBMS for comprehensive interview preparation.