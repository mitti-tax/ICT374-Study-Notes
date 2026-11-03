# Topic 09: Memory Management

## Overview
The OS must subdivide memory among many processes so that there is always a ready process for the CPU. The lecture goes from simple contiguous schemes (fixed and dynamic partitioning), to paging and segmentation, then to virtual memory (built on paging, segmentation, or both), the page-table variants, the TLB and cache, page replacement algorithms, and a quick look at UNIX/Solaris, Linux and Windows. Focus is virtual memory based on paging (one-level, two-level and inverted page tables, TLB, caching, replacement).

## Memory management requirements (slides 4-8)
- **Relocation**: the programmer does not know where the program will sit; it may be swapped out and back in at a different place. Memory references in code must be translated to real physical addresses.
- **Protection**: one process must not touch another's memory without permission. Absolute addresses cannot be checked at compile time, so checks happen at run time, and must be done by hardware (the OS cannot predict every reference).
- **Sharing**: several processes can access the same memory (e.g. one shared copy of a program).
- **Logical organization**: programs are written as modules, compiled independently, given different protection (read-only, execute-only) and shared.
- **Physical organization**: memory may be too small for program plus data; overlaying lets modules share the same region. The programmer cannot know how much space will be free.

## Fixed partitioning (slides 9-11)
- Memory split into fixed partitions, **equal-size** or **unequal-size** (Fig 7.2, 64 MB example: 8 x 8M, or 2M, 4M, 6M, 8M, 8M, 12M, 16M after the 8M OS).
- A process that fits (size <= partition) can load into a free partition; if all are full the OS swaps one out. A program too big needs **overlays**.
- **Internal fragmentation**: any program, however small, occupies a whole partition, wasting the rest.

## Dynamic partitioning (slides 12-13)
- Partitions have variable length and number; each process gets exactly what it needs.
- Over time, processes leave and holes appear: **external fragmentation** (Fig 7.4 shows the sequence of loading and removing processes leaving scattered holes).
- **Compaction** shifts processes so free memory is one contiguous block (costly, and relocates programs).

## Relocation and addresses (slides 14-18)
- A process's absolute addresses change after swapping or compaction.
- **Logical address**: reference independent of where data sits in memory; must be translated.
- **Relative address**: relative to a known point (a type of logical address).
- **Physical (absolute) address**: actual main-memory location.
- Hardware support (Fig 7.8): **base register** (start of process) and **bounds register** (end). Relative address + base (adder) = absolute address; the comparator checks it against bounds; if outside, an interrupt goes to the OS. Both registers are set when the process is loaded/swapped in.

## Paging (slides 19-22, 24-25)
- Memory is split into small equal fixed-size **frames**; each process into equal-size **pages**. Pages go to any free frames (need not be contiguous; Figs 7.9/7.10 load processes A, B, C, D into 15 frames, with a free-frame list).
- OS keeps a **page table per process**: frame number for each page. A logical address = page number + offset.
- Example (slide 25): 16-bit address, 6-bit page number, 10-bit offset; page table maps page 1 to frame 000110, giving physical address 0001100111011110 (frame bits replace page bits, offset unchanged).
- Slide 24 (Fig 7.11): relative address 1502 in a 2700-byte process with 1K pages is page 1, offset 478; the last page shows internal fragmentation.

## Segmentation (slides 23-26)
- Segments may differ in length (up to a maximum). Address = segment number + offset. Similar to dynamic partitioning, but a program can have multiple segments.
- Slide 24: the same relative address 1502 is segment 1, offset 752 (segment 0 is 750 bytes, segment 1 is 1950 bytes).
- Translation (Fig 7.12b): segment number indexes the segment table, giving length and base; physical = base + offset. Example: 4-bit segment #, 12-bit offset.

## Virtual memory scheme (slides 27-34)
- A process can be split into pieces not contiguous in memory, and not all pieces need be loaded. References are translated dynamically at run time.
- **Resident set**: the portion of the process currently in main memory.
- When an address is needed that is not resident, an interrupt occurs: the process is **blocked**, the OS issues a disk read, another process is dispatched meanwhile, and on I/O completion an interrupt moves the process to **Ready**.
- Advantages: more processes in memory, so a Ready process is more likely; a process can be larger than all of main memory.
- **Real memory** = main memory; **virtual memory** = memory on disk (effective multiprogramming, frees the user from memory limits).
- **Thrashing**: swapping out a piece just before it is needed; the CPU spends its time swapping instead of executing.
- **Principle of locality**: references in a process cluster, so only a few pieces are needed over a short period and future needs can be guessed. This is why virtual memory works.
- Needs hardware paging/segmentation support plus OS software to move pages/segments between disk and memory.

## Virtual memory based on paging (slides 35-43)
- Each process has its own page table; each entry holds the frame number plus a **present (P) bit** (in memory or not) and a **modify (M) bit** (changed since loaded; if not, no write-back needed on swap-out). Fig 8.3: page number indexes the page table (found via a page-table pointer register), frame number + offset gives the physical address.
- A whole page table may be too big for main memory, so page tables are themselves stored in virtual memory; only part is resident.
- **Two-level page table (32-bit address)**: a 4 KB root page table (1024 entries) points to a 4 MB user page table (made of 4 KB pages of 1024 entries), covering a 4 GB address space (Fig 8.4). Virtual address split 10 bits | 10 bits | 12-bit offset (Fig 8.5): root index, then page-table index, then offset.
- **Inverted page table** (PowerPC, UltraSPARC, IA-64): the page-number part of the virtual address is hashed; the hash value points into the inverted table, which has one entry per physical frame. Entries hold page number, process ID, control bits, chain pointer (to follow hash collisions). Size is a fixed proportion of real memory regardless of the number of processes.

## Translation Lookaside Buffer (slides 44-50)
- Every virtual reference needs at least two memory accesses (page table entry, then data). The **TLB** is a high-speed cache of recently used page table entries.
- **TLB hit**: the frame number is read from the TLB and the real address formed. **TLB miss**: the page number indexes the process page table; if the page is not in main memory a **page fault** occurs (page read from disk, page replacement if memory is full); the TLB is then updated (flowchart Fig 8.8).
- Fig 8.9: direct mapping (index page table by page number) versus associative mapping (TLB compares the page number against all entries).
- Fig 8.10: the TLB gives the real address; the **cache** of main memory then checks it (tag + remainder): hit returns the value from cache, miss goes to main memory.

## Page size (slides 51-52)
- Small pages: many pages fit in memory, each close to recent references, so page faults are low. Larger pages contain locations further from recent references, so page faults rise. (Table 8.2 lists real page sizes, e.g. VAX 512 bytes, DEC Alpha 8 KB, Pentium 4 KB or 4 MB.)

## Virtual memory based on segmentation (slides 53-57)
- Segments may be unequal and dynamic; this simplifies growing data structures, allows independent recompilation, and suits sharing and protection.
- One segment table per process; each entry has **length** and **base address**, a present bit, and a modify bit (Fig 8.12 translation: base + offset d). Fig 8.14 shows protection between segments (a process may not access the dispatcher or another process's data, and may not branch outside its own segment).

## Combined paging and segmentation (slides 58-60)
- Paging is transparent to the programmer; segmentation is visible. Each segment is broken into fixed-size pages.
- Virtual address = segment number | page number | offset. Segment table entry: control bits, length, segment base. Page table entry: P, M, other control bits, frame number (Fig 8.13: segment table, then page table, then frame + offset).

## Replacement policy (slides 61-70)
- Decides which page is replaced when there is no free frame: the one least likely to be referenced soon; most policies predict from past behaviour.
- **Frame locking**: a locked frame cannot be replaced (OS kernel, control structures, I/O buffers); a lock bit per frame.
- **Optimal**: replace the page whose next reference is farthest in the future. Impossible in practice (needs knowledge of the future); a benchmark.
- **LRU**: replace the page unused for the longest time. Good by locality, but tagging every page with last-use time is heavy overhead.
- **FIFO**: frames treated as a circular buffer, oldest page replaced round-robin. Simplest, but may evict pages needed again soon.
- **Clock**: FIFO plus a **use bit**. The bit is 1 when a page is loaded or referenced. To replace, a pointer scans circularly; the first frame with use 0 is replaced; each use bit of 1 passed over is cleared to 0. (Fig 8.16: use bits of pages 45 and 191 are cleared, page 556 with use 0 is replaced by page 727.)
- Fig 8.15 runs OPT, LRU, FIFO and CLOCK on the same page stream (2 3 2 1 5 2 4 5 3 2 5 2) with 3 frames, marking faults with F. Fig 8.17: page faults per 1000 references fall as more frames are allocated; FIFO is worst, then CLOCK, LRU, and OPT is best.

## UNIX and Solaris memory management (slides 71-75)
- Paging system data structures: **page table** (per process; entry has page frame number, age, copy on write, modify, reference, valid, protect), **disk block descriptor** (swap device number, device block number, type of storage), **page frame data table** (page state, reference count, logical device, block number, pfdata pointer), **swap-use table** (reference count, page/storage unit number) (Table 8.5, Fig 8.22).
- Page replacement is a refinement of the clock policy.

## Linux memory management (slides 76-77)
- Three-level page table: **page directory**, **page middle directory**, **page table**. Virtual address = global directory | middle directory | page table | offset; the `cr3` register points to the page directory (Fig 8.25).

## Windows memory management (slides 78-79)
- Page states: **available**, **reserved**, **committed**. Default virtual address space (Fig 8.26): 2 GB user space and 2 GB for the OS, with inaccessible 64 KB regions at address 0 (NULL pointers) and at the user/OS boundary (bad pointers).

## Key terms
- **Frame / page / segment**: physical fixed block / process fixed block / variable-size logical unit.
- **Internal fragmentation**: waste inside an allocated partition/page. **External fragmentation**: scattered free holes between partitions.
- **Compaction**: shifting processes to merge free space.
- **Base/bounds registers**: hardware relocation and protection.
- **Page table / segment table / inverted page table**: translation structures.
- **Resident set**: pages of a process in main memory.
- **Page fault**: referenced page is not in main memory.
- **Thrashing**: excessive swapping.
- **Locality**: references cluster in time and space.
- **TLB**: cache of page table entries.
- **Present (P), modify (M), use bit**: control bits in entries.
- **OPT, LRU, FIFO, Clock**: replacement algorithms.

## Questions
[Practice questions](../questions/topic-09-questions.md)

## Sources
- Lecture slides ICT374Topic09: slides 1-2 (objectives, reading), 3-8 (requirements), 9-13 (partitioning), 14-18 (relocation, addresses, registers), 19-22, 24-25 (paging), 23, 26 (segmentation), 27-34 (virtual memory, locality, thrashing), 35-43 (paging tables), 44-50 (TLB, cache), 51-52 (page size), 53-57 (segmentation VM), 58-60 (combined), 61-70 (replacement), 71-75 (UNIX), 76-77 (Linux), 78-79 (Windows).
- Stallings, Operating Systems, Ch 7 (pp. 340-359) and Ch 8, 8.1-8.5 (pp. 371-419); figures cited are Stallings Figs 7.2-7.12 and 8.3-8.26.
