# Operating Systems Complete Study Guide - GATE 2026

## Part 1: System Calls

### 1.1 What are System Calls?

A **system call** is an interface that allows user-mode programs to request services from the kernel (operating system). It serves as the boundary between user programs and privileged kernel operations.

**Key Points:**
- User programs execute in restricted "user mode"
- OS kernel executes in privileged "kernel mode"
- System calls provide controlled gateway to OS services
- Examples: file operations, process creation, memory allocation

### 1.2 Categories of System Calls

**1. File Management**
- open(), close(), read(), write()
- create(), delete(), rename()
- seek(), tell(), stat()

**2. Process Control**
- fork() - Create child process
- exec() - Replace process image
- wait() - Wait for child completion
- exit() - Terminate process
- kill() - Send signal to process

**3. Device Management**
- open(), close(), read(), write() (device-specific)
- ioctl() - Device-specific control

**4. Inter-Process Communication**
- pipe() - Create pipe between processes
- socket() - Create network socket
- shmget() - Allocate shared memory
- semget() - Create/access semaphore
- msgget() - Create message queue

**5. Information Maintenance**
- getpid() - Get process ID
- getppid() - Get parent process ID
- time() - Get current time
- sleep() - Sleep for specified duration

### 1.3 System Call Execution Flow

```
1. User program calls system call function
2. CPU switches to kernel mode
3. Kernel executes system call
4. Kernel performs requested operation
5. Kernel returns result to user program
6. CPU switches back to user mode
7. Execution continues in user program
```

---

## Part 2: Processes and Threads

### 2.1 Process Definition

A **process** is an instance of a program in execution with:
- Unique Process ID (PID)
- Memory space (code, data, heap, stack)
- Process control block (PCB)
- Associated resources (files, signals, etc.)

### 2.2 Process States

**Five states:**

```
┌─────────────────────────────────┐
│   NEW (Creation)                │
└────────────┬────────────────────┘
             │ Admit
             ▼
┌─────────────────────────────────┐
│   READY (Waiting for CPU)       │─────────────┐
└────────────┬────────────────────┘             │
             │ Dispatch                         │
             ▼                                   │
┌─────────────────────────────────┐  Preempt   │
│   RUNNING (Using CPU)           │─────────────┘
└────────────┬────────────────────┘
             │ I/O or Wait
             ▼
┌─────────────────────────────────┐
│   BLOCKED/WAITING                │
│   (Waiting for I/O/Event)        │
└────────────┬────────────────────┘
             │ I/O Completed
             ▼
┌─────────────────────────────────┐
│   READY                          │
└────────────┬────────────────────┘
             │
             ▼ All tasks done / Exit
┌─────────────────────────────────┐
│   TERMINATED                     │
└─────────────────────────────────┘
```

### 2.3 Process Control Block (PCB)

**Contains:**
- Process identification: PID, PPID
- Processor state: PC (Program Counter), Registers
- Memory info: Base address, Limit
- File info: Open file descriptors
- Accounting: CPU time, Memory used

### 2.4 Context Switching

**Definition:** Saving the state of running process and loading state of next process.

**Steps:**
1. Save context of current process to PCB
   - Save registers, program counter
   - Save memory management data
2. Update PCB with process state
3. Select next process from ready queue
4. Load context of next process from its PCB
5. Update memory management
6. Resume execution at saved program counter

**Context Switch Overhead:**
- Time to save/load state
- Memory management updates
- Reduced cache effectiveness (TLB flush)
- Typical: 1-1000 microseconds

### 2.5 Threads vs Processes

| Feature | Process | Thread |
|---------|---------|--------|
| Memory | Separate memory space | Shared memory (within process) |
| Creation | Expensive | Lightweight |
| Context Switch | Heavy | Light |
| Communication | IPC needed | Shared memory |
| Crash Impact | Isolated | All threads affected |
| Creation Time | Slow | Fast |

**Thread Components (within process):**
- Thread ID
- Program counter
- Register set
- Stack
- (Shared: heap, code, data segments, open files)

### 2.6 Process Creation

**fork() System Call:**
```c
pid = fork();
if (pid == 0) {
    // Child process (pid = 0)
} else if (pid > 0) {
    // Parent process (pid = child PID)
} else {
    // Error
}
```

**exec() System Call:**
- Replaces process image
- Child typically calls exec() after fork()
- Original program replaced entirely

**Example: Shell**
```
fork() - create child
wait() - parent waits
exec() - child runs new program
```

---

## Part 3: Inter-Process Communication (IPC)

### 3.1 Shared Memory

**Characteristics:**
- Fastest IPC method
- Both processes access same memory region
- Requires synchronization (semaphores/mutexes)
- Processes communicate by reading/writing shared memory

**Steps:**
1. Create shared memory segment
2. Attach segment to process
3. Read/write to shared memory
4. Detach when done

### 3.2 Message Passing

**Characteristics:**
- OS manages message delivery
- Processes don't share memory
- Can be synchronous or asynchronous
- Safer but slower than shared memory

**Types:**
- Direct: Sender names recipient
- Indirect: Via mailbox/queue
- Blocking: Wait for reply
- Non-blocking: Continue execution

### 3.3 Pipes

**Anonymous Pipe:**
- One-way communication channel
- Parent creates for child
- Used for output redirection: `command1 | command2`

**Named Pipe (FIFO):**
- Two-way communication
- Persistent in file system
- Processes reference by filename

**Characteristics:**
- FIFO order (First In, First Out)
- Buffered by OS
- Blocking by default

### 3.4 Signals

**Asynchronous notification to process:**
- 32+ signal types
- Examples: SIGKILL, SIGSTOP, SIGUSR1, SIGCHLD
- Handler or default action

---

## Part 4: Concurrency and Synchronization

### 4.1 Critical Section Problem

**Definition:** Code section where shared resources are accessed

**Requirements for Solution:**
1. **Mutual Exclusion**: Only one process in critical section
2. **Progress**: Process outside critical section cannot block others
3. **Bounded Waiting**: No starvation (eventual entry guaranteed)

### 4.2 Semaphores

**Binary Semaphore:**
- Value: 0 or 1
- Used for mutual exclusion
- P (wait): Decrement, block if < 0
- V (signal): Increment, wake one waiting process

**Counting Semaphore:**
- Non-negative integer
- Used for resource pool management
- Multiple resources can be accessed

**Implementation:**
```
P(S):
  S.value--
  if (S.value < 0) {
    Add process to S.queue
    Block process
  }

V(S):
  S.value++
  if (S.value <= 0) {
    Remove process from S.queue
    Wake process
  }
```

### 4.3 Mutex (Mutual Exclusion Lock)

**Characteristics:**
- Binary lock (locked/unlocked)
- Simpler than semaphore
- Lock ownership (only owner can unlock)
- No nested locks

**Usage:**
```
lock(mutex)
// Critical section
unlock(mutex)
```

### 4.4 Condition Variables

**Wait/Signal Pattern:**
```
lock(mutex)
while (condition not met) {
  wait(cond_var, mutex)  // Release mutex, wait for signal
}
// Do work
signal(cond_var)  // Wake one waiter
unlock(mutex)
```

**Producer-Consumer Example:**
```
Producer:
  lock, add item, signal consumers, unlock

Consumer:
  lock, wait while empty, remove item, signal producers, unlock
```

### 4.5 Monitor

**Language-level synchronization:**
- Encapsulates shared data
- Mutual exclusion automatic
- Condition variables for coordination
- Used in Java synchronized methods

---

## Part 5: Deadlock

### 5.1 Deadlock Conditions

**All four must be present:**

1. **Mutual Exclusion**: Resources cannot be shared
2. **Hold and Wait**: Process holds resources while waiting for others
3. **No Preemption**: Resources cannot be forcibly taken
4. **Circular Wait**: Chain of processes waiting for each other's resources

### 5.2 Deadlock Prevention

**Violate one condition:**

**1. Violate Mutual Exclusion:**
- Make resources sharable (rarely possible)
- Not practical for most resources

**2. Violate Hold and Wait:**
- All-or-nothing: Request all resources upfront
- OR: Release before requesting new ones

**3. Violate No Preemption:**
- Force preemption: Take resources from process
- Cost: Rollback and restart
- Used in CPU scheduling, not I/O

**4. Violate Circular Wait:**
- Impose total ordering on resources
- Request resources in order only
- No backward requests
- **Most practical approach**

### 5.3 Deadlock Avoidance

**Banker's Algorithm:**
- Check if request leads to unsafe state
- Only grant if state remains safe
- Requires knowing max resource needs

**Safety Check:**
```
Can all processes complete with available + held resources?
If yes: SAFE state, grant request
If no: UNSAFE state, deny request
```

### 5.4 Deadlock Detection

**Resource Allocation Graph:**
- Nodes: Processes and Resources
- Edges: Request (P→R), Assignment (R→P)
- Cycle: Deadlock exists (single-instance resources)

**Algorithm:**
1. Build resource allocation graph
2. Check for cycles
3. If cycle exists: Deadlock detected

**Multiple Instance Resources:**
- Use Wait-for Graph (process-to-process edges)
- Apply cycle detection

### 5.5 Deadlock Recovery

**Methods:**
1. **Process Termination**: Kill one/all processes
2. **Resource Preemption**: Take resource from process
3. **Rollback**: Restore to checkpoint before deadlock

---

## Part 6: CPU Scheduling

### 6.1 Scheduling Metrics

**Turnaround Time (TT):**
- Time from submission to completion
- TT = Completion Time - Arrival Time

**Waiting Time (WT):**
- Time waiting in ready queue
- Does not include I/O wait
- WT = TT - Burst Time

**Response Time (RT):**
- Time from submission to first response
- Important for interactive systems

**CPU Utilization:**
- Percentage time CPU is busy
- Goal: Maximize (> 40% good, > 80% very good)

### 6.2 FCFS (First Come, First Served)

**Characteristics:**
- Non-preemptive
- Processes execute in arrival order
- Simple implementation
- No starvation

**Disadvantage:**
- Can result in high waiting time
- Convoy effect (short jobs wait for long jobs)

**Example:**
```
Processes: A(10), B(1), C(1)
FCFS: A runs 10 time units, then B(1), then C(1)
Waiting Times: A=0, B=10, C=11
Average WT = 7
```

### 6.3 SJF (Shortest Job First)

**Characteristics:**
- Non-preemptive
- Process with shortest burst time selected first
- **Minimum average waiting time**

**Disadvantage:**
- **Starvation**: Long jobs wait indefinitely
- Prediction difficult for interactive systems

**Example:**
```
Processes: A(10), B(1), C(1)
SJF: B(1), C(1), A(10)
Waiting Times: B=0, C=1, A=2
Average WT = 1 (vs 7 for FCFS!)
```

### 6.4 SRTF (Shortest Remaining Time First)

**Characteristics:**
- **Preemptive version of SJF**
- Process with shortest remaining time selected
- Better than SJF for new arrivals

**Disadvantage:**
- More overhead (preemption)
- Starvation still possible

### 6.5 Priority Scheduling

**Characteristics:**
- Each process assigned priority
- Highest priority selected first
- Can be preemptive or non-preemptive

**Types:**
- **Static Priority**: Set at creation
- **Dynamic Priority**: Changes during execution

**Problem:**
- **Starvation**: Low priority processes may never run
- **Solution**: Aging - increase priority over time

### 6.6 Round Robin (RR)

**Characteristics:**
- **Preemptive**
- Each process gets time quantum (time slice)
- If not completed, moved to end of queue
- **Fair** - all processes get equal CPU time

**Time Quantum Effect:**
- Too small: Many context switches (overhead)
- Too large: Approaches FCFS

**Example (quantum=4):**
```
Processes: A(10), B(5), C(8)
Timeline: A(4), B(4), C(4), A(4), B(1), C(4)
All complete with fair allocation
```

**Advantages:**
- Good response time (interactive systems)
- Fair
- No starvation

### 6.7 Multilevel Queue Scheduling

**Multiple queues by priority:**
- System processes (highest)
- Interactive processes
- Batch processes (lowest)

**Scheduling within queue + between queues:**
- Each queue uses different algorithm
- Fixed portion of CPU per queue
- OR: Queues schedule until empty

### 6.8 Multilevel Feedback Queue

**Dynamic priority adjustment:**
- Process moves between queues
- High CPU use → lower queue (lower priority)
- Waiting long → higher queue (higher priority)
- Balances responsiveness and fairness

---

## Part 7: Memory Management

### 7.1 Virtual Memory Concept

**Virtual Address Space:**
- Logical view of memory
- Larger than physical memory
- Divided into pages (paging) or segments (segmentation)

**Address Translation:**
- Virtual Address → Physical Address
- Done by Memory Management Unit (MMU)
- Page table stores mappings

**Benefits:**
- Programs larger than RAM
- Protection between processes
- Shared memory possible

### 7.2 Paging

**Division into Pages:**
- Virtual memory divided into equal-sized pages
- Physical memory divided into frames
- Allocate pages to any free frames

**Page Table:**
- Virtual page number → Physical frame number
- One entry per page in virtual address space
- Can be very large for large address spaces

**Advantages:**
- No external fragmentation
- Easy to allocate

**Disadvantages:**
- Internal fragmentation (last page not full)
- Page table overhead
- Multi-level lookup expensive (solved by TLB)

### 7.3 Segmentation

**Logical Division:**
- Virtual address space divided into logical segments
- Each segment contiguous in physical memory
- Segments can be different sizes

**Address:** Segment ID + Offset
- Segment table maps to physical address

**Advantages:**
- Logical organization
- Support shared segments
- Easy protection per segment

**Disadvantages:**
- External fragmentation
- Complex allocation

### 7.4 Paging with Segmentation

**Combines benefits:**
- Segment table points to page tables
- Page tables contain frame addresses
- Two-level translation

### 7.5 Translation Lookaside Buffer (TLB)

**Purpose:** Cache for page table entries

**Characteristics:**
- Small, fast cache
- Stores recent virtual→physical translations
- Parallel search (associative)

**Operation:**
```
1. CPU generates virtual address
2. TLB searched in parallel
3. TLB Hit: Physical address returned immediately
4. TLB Miss: Page table consulted, TLB updated
```

**Performance:**
- TLB hit rate important (90-99% typical)
- Hit time: 1-2 clock cycles
- Miss time: 50-100 clock cycles

**TLB Flush:**
- On context switch (can lose efficiency)
- Advanced systems: ASID (per-process ID) avoids flush

### 7.6 Page Replacement Algorithms

**When page fault occurs:**
1. Find free frame (if available)
2. If none, select page to replace
3. Write victim page to disk
4. Load requested page
5. Update page table
6. Restart instruction

**FIFO (First-In-First-Out):**
- Replace oldest page
- Simple to implement
- Performance: Mediocre
- **Belady's Anomaly**: More frames can mean more faults

**LRU (Least Recently Used):**
- Replace page used least recently
- Good performance (locality of reference)
- Implementation: Keep usage order
- Overhead: Track all accesses

**Optimal (Bélády's Algorithm):**
- Replace page used furthest in future
- **Best performance** (minimum page faults)
- Impractical: Requires knowing future
- Used as theoretical upper bound

**Clock Algorithm:**
- Approximation of LRU
- Circular buffer with use bit
- Pointer sweeps through
- Replace page with use bit = 0

**Performance Comparison:**
```
Reference string: 1,2,3,4,1,2,5,1,2,3,4,5
With 3 frames:

FIFO:  9 page faults
LRU:   8 page faults
OPT:   7 page faults (best)
```

### 7.7 Thrashing

**Definition:** Excessive paging, system spends more time in I/O than execution

**Cause:**
- Too many processes competing for memory
- Page faults trigger I/O
- Process swapped out before completing
- New process starts, triggers more faults

**Detection:**
- High page fault rate
- Low CPU utilization (despite high load)

**Prevention:**
- Limit concurrency
- Use working set model
- Allocate minimum frames per process

---

## Part 8: I/O Scheduling

### 8.1 Disk Scheduling

**Goal:** Minimize seek time (time to move head to correct track)

**Seek Time Formula:**
```
Seek time ∝ |Current track - Request track|
```

### 8.2 FCFS (First Come, First Served)

**Characteristics:**
- Process requests in order received
- Simple implementation
- **High seek time**
- Fair

**Example:**
```
Requests: 55, 58, 39, 18, 90, 160, 150, 38, 184
Starting at 100:
100→55 (45) →58 (3) →39 (19) →18 (21) →90 (72) →160 (70) →150 (10) →38 (112) →184 (146)
Total seek: 398
```

### 8.3 SSTF (Shortest Seek Time First)

**Characteristics:**
- Select request closest to current position
- **Minimizes seek time** better than FCFS
- **Can cause starvation** (middle tracks favored)

**Example (same requests):**
```
Starting at 100:
100→90 (10) →55 (35) →39 (16) →38 (1) →18 (20) →150 (132) →160 (10) →184 (24) →58 (126)
Total seek: 374 (better than FCFS)
```

### 8.4 SCAN (Elevator Algorithm)

**Characteristics:**
- Head moves in one direction servicing requests
- At end, reverses direction
- **Uniform wait time**
- **Inner tracks might get priority**

**Example:**
```
Starting at 100, moving up:
100→150 (50) →160 (10) →184 (24) [end] →90 (94) →58 (32) →55 (3) →39 (16) →38 (1) →18 (20)
Total seek: 250 (good)
```

### 8.5 C-SCAN (Circular SCAN)

**Characteristics:**
- Like SCAN but returns to start without servicing
- More uniform distribution
- Never serves inner tracks twice

**Example:**
```
Moving up: 100→150→160→184 [return to 18]
Then: 18→38→39→55→58→90
Total different path pattern
```

### 8.6 LOOK and C-LOOK

**LOOK:**
- Modification of SCAN
- Don't go to end if no requests
- Stop at last request then reverse

**C-LOOK:**
- Modification of C-SCAN
- Stop at last request in direction

---

## Part 9: File Systems

### 9.1 File Structure

**File:** Named collection of data on disk

**File Operations:**
- Create, Delete, Open, Close
- Read, Write, Seek, Append
- List, Rename, Copy

### 9.2 File Allocation Methods

**Contiguous Allocation:**
- File occupies continuous blocks
- Fast access (direct calculation of block address)
- External fragmentation
- **Not used in modern systems** (difficult to extend)

**Linked Allocation:**
- Each block contains pointer to next
- Scattered blocks
- Free block management easy
- Random access slow
- **Example: FAT filesystem**

**Indexed Allocation:**
- Index block contains pointers to file blocks
- Fast access
- Internal fragmentation (last block)
- **Example: Unix inodes**

### 9.3 Free Space Management

**Bitmap:**
- Bit per block (1=free, 0=used)
- Efficient for finding free blocks
- Small space overhead

**Linked List:**
- Free blocks linked together
- Traversal time to find free block
- Larger overhead

**Grouping:**
- Group of free blocks stored in first block
- Efficient for contiguous allocation

---

## Part 10: CPU and I/O Performance Calculations

### 10.1 Average Waiting Time

**Formula:**
```
AWT = (Sum of all waiting times) / (Number of processes)
WT(Process) = Completion Time - Arrival Time - Burst Time
```

### 10.2 Average Turnaround Time

```
ATT = (Sum of all turnaround times) / (Number of processes)
TT(Process) = Completion Time - Arrival Time
```

### 10.3 CPU Utilization

```
CPU Utilization = (Total CPU time) / (Total elapsed time) × 100%
```

### 10.4 Context Switch Time

**Measured in microseconds**
- Save registers: ~0.1 μs
- Load registers: ~0.1 μs
- Memory management update: ~0.1-10 μs
- **Total: 1-1000 μs typical**

---

## Study Strategy for GATE 2026

### Phase 1: Fundamentals (Weeks 1-2)
- System calls and processes
- Process states and context switching
- PCB structure

### Phase 2: Concurrency (Weeks 3-4)
- Synchronization primitives (semaphore, mutex)
- Critical sections
- Deadlock conditions and prevention

### Phase 3: Scheduling (Weeks 5-6)
- All CPU scheduling algorithms
- Scheduling metrics calculation
- Comparison and analysis

### Phase 4: Memory Management (Weeks 7-8)
- Paging and segmentation
- Virtual memory
- Page replacement algorithms

### Phase 5: I/O and File Systems (Weeks 9-10)
- Disk scheduling algorithms
- File allocation methods
- Free space management

### Phase 6: Practice (Weeks 11-14)
- Solve 30+ practice problems
- Solve previous year GATE questions
- Focus on weak areas
- Mock exams

---

## High-Weightage Topics Priority

**Tier 1 (Must Know):**
1. Process scheduling (FCFS, SJF, RR, Priority)
2. Synchronization (Semaphore, Mutex, Deadlock)
3. Virtual memory and paging
4. Process and thread concepts

**Tier 2 (Very Important):**
5. Deadlock prevention/detection
6. Page replacement algorithms
7. CPU scheduling calculations
8. Context switching

**Tier 3 (Important):**
9. I/O scheduling
10. File systems
11. IPC mechanisms

---

**Total Study Hours Recommended: 140-160 hours**

Maximum GATE marks in OS: 15-20 out of 100 = 6-8 questions

Good luck with your preparation!
