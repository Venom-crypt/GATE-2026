# Operating Systems - GATE 2026 Quick Reference Sheet

## CPU Scheduling Algorithms Comparison

| Algorithm | Type | Preemptive | WT | TT | Starvation | Implementation |
|-----------|------|-----------|-----|-----|-----------|-----------------|
| **FCFS** | Batch | No | High | High | No | Easy |
| **SJF** | Batch | No | Minimum | Low | **Yes** | Hard (predict) |
| **SRTF** | Batch | Yes | Medium | Medium | **Yes** | Hard |
| **Priority** | General | Yes/No | Variable | Variable | **Yes** | Medium |
| **Round Robin** | Interactive | Yes | Good | Good | No | Easy |
| **HRRN** | Batch | No | Low | Low | No | Complex |

**Key Formulas:**
- WT = Completion Time - Arrival Time - Burst Time
- TT = Completion Time - Arrival Time
- TT = WT + Burst Time

---

## Process Scheduling Calculation Example

**Processes: A(5), B(3), C(8), D(6) arriving at t=0**

### FCFS
```
A(5) B(3) C(8) D(6)
WT: A=0, B=5, C=8, D=16 | Average = 7.25
TT: A=5, B=8, C=16, D=22 | Average = 12.75
```

### SJF
```
B(3) A(5) D(6) C(8)
WT: B=0, A=3, D=8, C=14 | Average = 6.25
TT: B=3, A=8, D=14, C=22 | Average = 11.75
```

### Round Robin (quantum=3)
```
A(3) B(3) C(3) D(3) A(2) C(5) D(3)
Calculate each process WT and TT individually
```

---

## Disk Scheduling (Seek Time Comparison)

| Algorithm | Seek Time | Starvation | Fairness | Use Case |
|-----------|-----------|-----------|----------|----------|
| **FCFS** | High | No | Fair | Random requests |
| **SSTF** | Low | **Yes** | Unfair | Heavy load |
| **SCAN** | Medium | No | Fair | General purpose |
| **C-SCAN** | Medium | No | Fair | Better distribution |
| **LOOK** | Medium | No | Fair | Optimized SCAN |

**Calculation:** Total seek = Sum of |Current track - Next track|

---

## Page Replacement Algorithms

| Algorithm | Implementation | Performance | Overhead |
|-----------|-----------------|-----------|----------|
| **FIFO** | Queue | Mediocre | Low |
| **LRU** | Stack/Heap | Good | High |
| **Optimal** | Future knowledge | Best | N/A (theoretical) |
| **Clock** | Circular buffer | Good | Low |
| **Aging** | Counter update | Good | Medium |

**Reference String Example:**
```
String: 1,2,3,4,1,2,5,1,2,3,4,5 (Frames = 3)

FIFO:    3 4 1 → 3 4 5 → 1 2 5 → 1 2 3 → 1 2 4 → 1 2 5
Faults:  1 2 3    4       5 6      7       8       9

LRU:     Similar calculation tracking usage order
Faults:  Generally fewer than FIFO (depends on access pattern)

OPT:     Looks ahead, replaces future-furthest page
Faults:  Minimum possible
```

---

## Deadlock Conditions and Prevention

**Four Necessary Conditions:**
1. **Mutual Exclusion** - Resources cannot be shared
2. **Hold and Wait** - Hold resource while requesting others
3. **No Preemption** - Cannot forcibly take resources
4. **Circular Wait** - Chain of processes waiting

**Prevention (Violate one):**
| Condition | Prevention | Cost |
|-----------|-----------|------|
| Mutual Exclusion | Make shareable | N/A for most resources |
| Hold and Wait | Request all at once | Low utilization |
| No Preemption | Force preemption | Rollback overhead |
| Circular Wait | Order resources | Simple, most practical |

**Detection (Single Instance):**
- Check for cycle in Resource Allocation Graph
- Cycle = Deadlock

**Detection (Multiple Instances):**
- Use Wait-for Graph
- Apply cycle detection
- Or use safety algorithm

---

## Synchronization Primitives

### Semaphore
```
P(S):  S.count--; if (S.count < 0) block;
V(S):  S.count++; if (S.count <= 0) wake();
```

### Mutex (Binary Semaphore)
```
lock(mutex)
// Critical section
unlock(mutex)
```

### Monitor (Language-level)
```
synchronized method {
  while (condition not met) wait();
  // Work
  notifyAll();
}
```

**Synchronization Patterns:**
- Mutual Exclusion: Binary semaphore/mutex
- Signaling: Counting semaphore
- Barrier: Counting semaphore
- Producer-Consumer: Two semaphores (full, empty)

---

## Memory Management Quick Facts

### Virtual Memory
- Allows programs larger than RAM
- Uses paging or segmentation
- Requires address translation

### Paging
- Fixed-size pages (typically 4KB)
- No external fragmentation
- Internal fragmentation per page
- Multi-level lookup expensive (TLB helps)

### Segmentation
- Variable-size segments
- Logical organization
- External fragmentation
- Easier protection

### TLB (Translation Lookaside Buffer)
- Cache for page table entries
- Hit time: 1-2 cycles
- Miss time: 50-100 cycles
- Hit rate: 90-99% typical

### Page Fault Overhead
```
Soft page fault: ~100 μs (frame in RAM)
Hard page fault: ~5-10 ms (disk read required)
Context switch: 1-1000 μs
```

---

## File Allocation Methods

| Method | Speed | Fragmentation | Access | Update |
|--------|-------|----------------|--------|--------|
| **Contiguous** | Fast | External | Fast | Slow |
| **Linked** | Slow | None | Slow (sequential) | Fast |
| **Indexed** | Fast | Internal | Fast | Medium |

### Calculations:
```
Contiguous:
- First block address: F
- File size: S blocks
- Block B is at: F + B

Linked:
- Follow pointers (sequential only)
- Complex for random access

Indexed:
- Direct calculation like contiguous
- Index block overhead
```

---

## Context Switch Overhead

**Components:**
- Save registers: ~0.1 μs
- Update memory management: ~0.1-10 μs
- Load new context: ~0.1 μs
- TLB flush (if needed): ~100-1000 μs
- **Total: 1-1000 μs typical**

**Impact:**
- Round Robin with small quantum: Many switches, high overhead
- Too few switches: Poor responsiveness
- Optimal: Balance between overhead and responsiveness

---

## Banker's Algorithm (Deadlock Avoidance)

**Steps:**
1. Check if request exceeds available + process allocated
2. If yes, deny (would exceed max)
3. If no, tentatively allocate
4. Run safety algorithm:
   - Can all processes complete with current state?
   - If yes: Safe state, grant request
   - If no: Unsafe state, deny request

**Calculation:**
```
Need[i][j] = Max[i][j] - Allocation[i][j]
Available[j] = Total[j] - Sum(Allocation[*][j])
```

---

## System Call Categories and Examples

| Category | Examples | Purpose |
|----------|----------|---------|
| **File** | open, close, read, write | File operations |
| **Process** | fork, exec, wait, exit | Process management |
| **Device** | open, close, read, write | Device control |
| **IPC** | pipe, socket, shmget, msgget | Inter-process communication |
| **Info** | getpid, getppid, time, sleep | Information retrieval |

---

## Process States and Transitions

```
NEW → READY (Admit)
↓
READY ↔ RUNNING (Dispatch/Preempt)
↓
RUNNING → BLOCKED (I/O request)
↓
BLOCKED → READY (I/O complete)
↓
RUNNING → TERMINATED (Exit)
```

**Context switch occurs on:**
- I/O request (RUNNING → BLOCKED)
- I/O completion (BLOCKED → READY)
- Timer interrupt (RUNNING → READY, preemption)
- Process termination

---

## Thread vs Process Key Differences

| Feature | Process | Thread |
|---------|---------|--------|
| Memory | Separate | Shared |
| Creation | Slow (~100ms) | Fast (~1ms) |
| Context Switch | Slow (~1-1000μs) | Fast (~100ns) |
| IPC | Complex | Simple (shared memory) |
| Crash | Isolated | All affected |

---

## Memory Calculation Examples

### Address Translation (Paging)
```
Virtual Address: 32 bits
Page Size: 4KB (4096 bytes = 2^12)
Offset bits: 12 (for 4KB)
Page number bits: 32 - 12 = 20 bits
Max pages: 2^20 = 1M pages

Physical Memory: 1GB = 2^30 bytes
Frame number bits: 30 - 12 = 18 bits
Max frames: 2^18 = 256K frames
```

### Seeking Time
```
Current track: 50
Request track: 150
Seek time ∝ |150 - 50| = 100

With multiple requests:
Total seek = Σ |Current - Next|
```

---

## High-Frequency GATE Topics

1. **Scheduling Calculations** (Average WT, TT)
2. **Deadlock Detection** (Resource graphs, cycles)
3. **Page Replacement** (Fault counting)
4. **Synchronization** (Semaphore analysis)
5. **Context Switching** (Time calculations)
6. **Address Translation** (Virtual → Physical)
7. **Disk Scheduling** (Seek time)
8. **File Allocation** (Block calculations)

---

## Common GATE Problem Patterns

**Pattern 1: Scheduling**
- Given process burst times and arrival times
- Calculate average waiting/turnaround time
- Compare algorithms

**Pattern 2: Deadlock**
- Build resource allocation graph
- Check for cycles
- Determine if deadlock exists

**Pattern 3: Paging**
- Given reference string and frame count
- Count page faults for each algorithm
- Compare performance

**Pattern 4: Synchronization**
- Analyze semaphore code
- Check for deadlock
- Verify mutual exclusion

**Pattern 5: Memory**
- Address translation calculations
- Page table lookups
- Fragmentation calculations

---

## Quick Problem-Solving Checklist

**For Scheduling Problems:**
- [ ] Identify arrival times
- [ ] Calculate completion time for each process
- [ ] Calculate waiting time: CT - AT - BT
- [ ] Calculate turnaround time: CT - AT
- [ ] Average all metrics

**For Deadlock Problems:**
- [ ] Build resource allocation graph
- [ ] Look for cycles
- [ ] Cycle = Deadlock (single instance)
- [ ] Use wait-for graph (multiple instances)

**For Page Replacement:**
- [ ] Count page faults
- [ ] Track which pages in memory
- [ ] Compare algorithms

**For Synchronization:**
- [ ] Check mutual exclusion
- [ ] Check deadlock possibility
- [ ] Verify bounded waiting

**For Memory Calculations:**
- [ ] Convert addresses correctly
- [ ] Calculate offsets from page size
- [ ] Use correct formula for translation

---

Print this sheet for final day revision!
