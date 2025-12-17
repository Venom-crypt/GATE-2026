# Operating Systems - GATE 2026 Practice Problems

## Part 1: CPU Scheduling (5 Problems)

### Problem 1.1
Processes: P1(8), P2(4), P3(2), P4(1) - all arrive at t=0

Calculate average waiting time using:
a) FCFS b) SJF c) Round Robin (quantum=2)

**Solution:**
```
FCFS: P1(8) P2(4) P3(2) P4(1)
CT: 8, 12, 14, 15
WT: 0, 8, 12, 14 | Avg = 8.5

SJF: P4(1) P3(2) P2(4) P1(8)
CT: 1, 3, 7, 15
WT: 0, 1, 3, 7 | Avg = 2.75

RR(q=2): P1(2) P2(2) P3(2) P4(1) P1(2) P2(2) P1(2)
WT: 11, 8, 4, 1 | Avg = 6
```

### Problem 1.2
Five processes arrive with burst times and priorities. Calculate preemptive priority scheduling completion time and average WT.

### Problem 1.3
Two processes: A(10), B(5)
Arrival: A at t=0, B at t=3
Compare FCFS vs Preemptive SJF

**Solution:**
```
FCFS: A completes 10, B waits 7 + 5 = 12
WT: A=0, B=7 | Avg = 3.5

SRTF: A(3) → B(5) → A(7)
At t=3, B arrives with burst 5 < A's remaining 7
WT: A=5, B=0 | Avg = 2.5
```

### Problem 1.4
Convoy effect: Show why SJF is better than FCFS for mixed job sizes

### Problem 1.5
Time quantum effect in RR: Compare quantum=1, 2, 4, 8 for overhead and response time

---

## Part 2: Synchronization and Deadlock (6 Problems)

### Problem 2.1
Analyze code for race condition:
```
Process A: x = x + 1
Process B: x = x + 1
```
Initial x = 5. Possible final values?

**Solution:**
```
Possible interleaving:
A reads 5, B reads 5
A writes 6, B writes 6
Final: 6 (should be 7 - race condition!)

Possible values: 6 or 7
```

### Problem 2.2
Binary semaphore solution:
```
sem_t mutex = 1
P(mutex)
x = x + 1
V(mutex)
```

Prove mutual exclusion and bounded waiting

### Problem 2.3
Producer-Consumer with semaphores:
```
sem_t full = 0, empty = N, mutex = 1

Producer:
P(empty)
P(mutex)
produce
V(mutex)
V(full)

Consumer:
P(full)
P(mutex)
consume
V(mutex)
V(empty)
```

Verify no deadlock possible

### Problem 2.4
Deadlock detection with Resource Allocation Graph
- 3 processes, 3 resources
- P1 holds R1, requests R2
- P2 holds R2, requests R3
- P3 holds R3, requests R1

Is there a deadlock? (Yes - cycle P1→P2→P3→P1)

### Problem 2.5
Banker's Algorithm
```
Available: (3, 3, 2)
Max: [[7, 5, 3], [3, 2, 2], [9, 0, 2]]
Allocation: [[0, 1, 0], [2, 0, 0], [3, 0, 2]]
```
Process 1 requests (0, 2, 0). Grant or deny?

**Solution:**
Calculate Need matrix, available after grant, run safety algorithm

### Problem 2.6
Deadlock prevention by violating circular wait:
Design ordering of 4 resources to prevent circular wait

---

## Part 3: Memory Management (6 Problems)

### Problem 3.1
Address translation with paging:
```
Virtual Address: 12-bit
Physical Address: 10-bit
Page Size: 256 bytes

Translate VA: 0x456
```

**Solution:**
```
Page size = 256 = 2^8
Offset = 8 bits
Page number = 12 - 8 = 4 bits

VA 0x456 = 010001010110
Page: 0100 (4), Offset: 01010110 (86)

Assume page table: Page 4 → Frame 2
PA = (Frame 2)(Offset 86) = 0x256
```

### Problem 3.2
Segmentation address translation:
- 3 segments, each 4KB
- Segment 0 at physical 0x0000
- Segment 1 at physical 0x4000
- Segment 2 at physical 0x8000

Translate: Segment 1, Offset 0x500

**Solution:**
PA = 0x4000 + 0x500 = 0x4500

### Problem 3.3
Multi-level page tables:
```
32-bit VA, 4KB pages (12-bit offset)
First level: 10 bits (1024 entries)
Second level: 10 bits (1024 entries)

VA: 0xABCDEF12
```

Extract: First level index, second level index, offset

### Problem 3.4
Internal vs External Fragmentation:
```
Memory: 1GB
Allocation: Contiguous, Page size 4KB
File 1: 5KB, File 2: 3KB, File 3: 9KB
```

Calculate internal fragmentation

**Solution:**
```
File 1: Uses 2 pages = 8KB, waste = 3KB
File 2: Uses 1 page = 4KB, waste = 1KB
File 3: Uses 3 pages = 12KB, waste = 3KB
Total internal fragmentation = 7KB
```

### Problem 3.5
TLB performance calculation:
```
TLB hit rate: 95%
TLB hit time: 2 cycles
TLB miss time: 100 cycles
```

Calculate average access time

**Solution:**
AAT = 0.95 × 2 + 0.05 × (2 + 100) = 1.9 + 5.1 = 7 cycles

### Problem 3.6
Working set and thrashing:
```
Process working set: 50 pages
Available memory: 40 pages
```

Analyze thrashing occurrence and mitigation

---

## Part 4: Page Replacement (4 Problems)

### Problem 4.1
Page replacement algorithms comparison:
```
Reference string: 1,2,3,4,5,1,2,3,4,5,1,2
Frames available: 3

Calculate page faults for FIFO, LRU, OPT
```

**Solution:**
```
FIFO:
1 → 1 | 2 → 1 2 | 3 → 1 2 3 | 4 → 2 3 4 | 5 → 3 4 5 |
1 → 1 4 5 | 2 → 2 4 5 | 3 → 2 3 5 | 4 → 2 3 4 | 5 → 3 4 5 | 1 → 1 4 5 | 2 → 1 2 5
Faults: 9

LRU: (track usage)
Faults: 7-8

OPT: (predict future)
Faults: 6-7
```

### Problem 4.2
Belady's Anomaly:
```
Reference: 1,2,3,4,1,2,5,1,2,3,4,5

With 3 frames: 9 faults
With 4 frames: Check if more faults (anomaly)
```

FIFO exhibits this anomaly, LRU does not

### Problem 4.3
Page replacement with write bit:
```
Dirty pages in memory: 2 (cost to write back)
Clean pages in memory: 1 (no write back)

Compare LRU vs LRU with write bit preference
```

### Problem 4.4
Demand paging with pre-paging:
```
Working set: 5 pages
Page fault time: 10ms
Pre-paging overhead: 5ms

Decide if pre-paging beneficial
```

---

## Part 5: I/O Scheduling (4 Problems)

### Problem 5.1
Disk scheduling seek time calculation:
```
Requests: 55, 58, 39, 18, 90, 160, 150, 38, 184
Starting position: 100

Calculate total seek time for FCFS, SSTF, SCAN
```

**Solution:**
```
FCFS: 
|100-55| + |55-58| + |58-39| + |39-18| + |18-90| + |90-160| + |160-150| + |150-38| + |38-184| = 640

SSTF:
Much lower (select closest each time)

SCAN:
Moderate (sweeps in direction)
```

### Problem 5.2
SCAN vs C-SCAN:
```
Compare for 200 tracks, requests at 10, 50, 100, 150, 190
Starting at 75, moving up
```

SCAN goes to 200 then back, C-SCAN returns to 0 directly

### Problem 5.3
Starvation in disk scheduling:
```
Demonstrate how SSTF can starve track 150
while middle tracks (50-100) have requests
```

### Problem 5.4
Context of queue length effect:
```
With 10 pending requests vs 1 pending request
How does SSTF fairness change?
```

---

## Part 6: Context Switching (3 Problems)

### Problem 6.1
Context switch overhead calculation:
```
Save registers: 50 processes/second
Load registers: 50 processes/second
Memory update: 100 processes/second
Total switching capacity: ?
```

Calculate maximum context switches per second

### Problem 6.2
Impact on scheduling:
```
Round Robin with quantum = 10ms
Context switch time = 1ms

Calculate CPU waste percentage
```

**Solution:**
Switching overhead = 1/(10+1) ≈ 9% waste

### Problem 6.3
TLB flush cost:
```
Process A working set: 100 pages
Process B working set: 100 pages
TLB size: 64 entries

Context switching frequency?
```

---

## Part 7: File Systems (2 Problems)

### Problem 7.1
File allocation calculation:
```
Contiguous: File at block 100, size 10 blocks
Access block 5 of file

Linked: File starts at block 100
Access block 5 of file (follow pointers)

Indexed: Index block at 200, file data elsewhere
Access block 5 of file
```

Compare access time and complexity

### Problem 7.2
Free space management:
```
Disk with 100 blocks
Allocated: Blocks 0-10, 20-30, 50-60
Find block for new file needing 5 blocks

Using Bitmap and Linked List methods
```

---

## Answer Checklist

**Scheduling:**
- [ ] Calculated all completion times
- [ ] Applied correct formula for waiting time
- [ ] Compared results across algorithms
- [ ] Identified starvation cases

**Deadlock:**
- [ ] Drew resource allocation graph
- [ ] Checked for cycles
- [ ] Applied safety algorithm if needed
- [ ] Traced banker's algorithm steps

**Memory:**
- [ ] Converted addresses correctly
- [ ] Applied page table lookup
- [ ] Calculated fragmentation
- [ ] Used correct formulas

**Paging:**
- [ ] Counted page faults accurately
- [ ] Tracked page replacements
- [ ] Compared algorithms correctly

**I/O Scheduling:**
- [ ] Calculated cumulative seek times
- [ ] Applied algorithm correctly
- [ ] Compared total movements

Good luck!
