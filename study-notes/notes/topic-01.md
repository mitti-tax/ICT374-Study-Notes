# Topic 1: Computer System Overview

## Overview
The lecture reviews the hardware an OS runs on: processor, memory, I/O modules and the system bus. It then shows how the speed gap between components (CPU vs disk) is managed with two ideas. Interrupts stop the CPU waiting on slow devices and make preemptive multitasking possible. Caching (a memory hierarchy plus the principle of locality) gives a large, fast, affordable memory. Finally it compares three I/O techniques: programmed I/O, interrupt-driven I/O and DMA.

Goals (slides 2-4): know the hardware components, the roles of registers such as PC and PSW, how data flows over the buses, how interrupts work, the memory hierarchy and cache, and the three I/O methods.

## Basic elements
Four elements (slide 6):
- **Processor**: executes instructions and controls the computer.
- **Main memory**: volatile; also called real or primary memory.
- **I/O modules**: secondary memory devices, communications equipment, terminals.
- **System bus**: carries communication among processor, memory and I/O modules.

## Processor internals (slides 7-8)
Processor registers for talking to memory and I/O:
- **MAR** (memory address register): address of the next read or write.
- **MBR** (memory buffer register): data being written to, or just read from, memory.
- **I/O address register** and **I/O buffer register**: the same roles for I/O devices.

Top-level view (Fig 1.1): CPU (registers PC, IR, MAR, MBR, I/O AR, I/O BR, plus execution unit), main memory (numbered locations holding instructions and data), and an I/O module with buffers, all joined by the system bus.

## Processor registers (slides 9-14)
Two groups:
- **User-visible registers**: usable by machine-language programs (application and system). They let a programmer cut main-memory references.
  - Data registers.
  - Address registers: *index* (base value plus index gives an address), *segment pointer* (address = segment + offset), *stack pointer* (top of stack).
- **Control and status registers**: used by the processor to control its operation, and by privileged OS routines to control programs.
  - **PC** (program counter): address of the next instruction to fetch.
  - **IR** (instruction register): the most recently fetched instruction.
  - **PSW** (program status word): condition codes, interrupt enable/disable, supervisor/user mode.
  - **Condition codes (flags)**: bits in the PSW set by hardware after operations (positive, negative, zero, overflow).

**Word size** (slide 10): "32-bit" or "64-bit" means the word size, the amount of data one CPU register holds. It also matches the data bus width, so one word moves between processor and memory per transfer.

## Instruction execution (slides 15-18)
The instruction cycle has two steps: **fetch** the instruction from memory, then **execute** it.
- The PC holds the address of the next instruction; it is incremented after each fetch.
- The fetched instruction goes into the IR.
- Instruction categories: processor-memory (transfer data between CPU and memory), processor-I/O (to or from a peripheral), data processing (arithmetic/logic), control (alter the sequence of execution, e.g. jump).

## Interrupts (slides 19-34)
Why: most I/O devices are slower than the processor, so the processor would otherwise idle waiting. An interrupt breaks the normal sequencing and lets the CPU do other work (e.g. switch to another process) while I/O completes.

**Classes of interrupt** (slide 20):
- *Program*: from an instruction's result: arithmetic overflow, divide by zero, illegal instruction, reference outside the user's allowed memory.
- *Timer*: from a timer inside the processor; lets the OS do things regularly.
- *I/O*: from an I/O controller, for normal completion or an error.
- *Hardware failure*: power failure, memory parity error.

**Flow of control** (Fig 1.5, slides 21-23):
- Without interrupts: the program issues a write and waits for the whole I/O operation.
- With interrupts, short I/O wait: the CPU runs other code during the I/O, but the I/O finishes before the next write is needed, so the program only waits briefly.
- With interrupts, long I/O wait: the next write is reached before the first I/O finishes, so the program still has to wait, but part of the wait is overlapped. Timing diagrams (slides 28-29) show the same cases.

**Interrupt handler** (slide 24): the routine that services a device; generally part of the OS. An interrupt suspends the normal sequence of execution.

**Interrupt cycle** (slides 26-27): after each execute, the processor checks for interrupts. None pending: fetch the next instruction. Pending: suspend the current program and run the handler.

**Simple interrupt processing** (Fig 1.10, slides 30-31). Hardware: device signals the processor; the processor finishes the current instruction, acknowledges the device, pushes the PSW and PC onto the control stack, and loads the PC with the handler's entry address. Software: the handler saves other register state, processes the interrupt, restores registers, then returns (restoring PSW and PC) to the interrupted program. The slide-31 figure shows the stack and registers before and after.

**Multiple interrupts** (slides 32-34), two approaches:
- Disable interrupts while one is being processed (sequential handling; new ones stay pending).
- Define priorities: a higher-priority interrupt can interrupt a lower-priority handler (nested handling).

## Multiprogramming (slide 35)
The processor has more than one program to run. The order depends on priority and on which are waiting for I/O. After an interrupt handler finishes, control may not return to the program that was interrupted. Interrupts are what make preemptive multitasking work.

## Memory hierarchy (slides 36-39)
Going down the hierarchy (registers, cache, main memory, then secondary storage such as disk, CD/DVD, tape):
- cost per bit decreases
- capacity increases
- access time increases
- frequency of access by the processor decreases (this relies on **locality of reference**)

The trade-off: faster means more cost per bit; greater capacity means lower cost per bit but slower access.

**Secondary memory** (slide 39): non-volatile, auxiliary; stores program and data files.

**Disk cache** (slide 40): a portion of main memory used as a buffer for disk data. Disk writes can be clustered, and data written out but referenced again comes back quickly from the cache instead of the slow disk.

## Cache memory (slides 41-45)
- Invisible to the OS; speeds up memory access because the processor is faster than memory; exploits the principle of locality.
- Organisation (Fig 1.16): CPU to cache is word transfer; cache to main memory is block transfer. Can be a single cache or three levels (L1 fastest, then L2, L3, then main memory).
- Cache holds a copy of part of main memory. The processor checks the cache first. On a miss, the block containing the needed word is loaded into the cache and the word is delivered to the processor.
- Structure (Fig 1.17): main memory is divided into blocks; cache is a set of slots (lines), each with a tag identifying which block it holds.
- Read operation (Fig 1.18): the processor generates the address RA. If the block is in cache, deliver the word. If not, fetch the block from main memory into a cache slot, then deliver the word.

## I/O techniques (slides 46-49)
- **Programmed I/O**: the I/O module does the work, not the processor, and sets bits in its I/O status register. No interrupts occur. The processor polls the status until the operation completes (busy waiting).
- **Interrupt-driven I/O**: the processor is interrupted when the module is ready to exchange data. It saves the context of the running program and runs the handler. No needless waiting, but it uses a lot of processor time because every word passes through the processor.
- **Direct Memory Access (DMA)**: the processor grants the I/O module authority to read or write memory directly, relieving the processor of the exchange. A block is transferred to or from memory, an interrupt is sent when the transfer completes, and the processor continues other work in the meantime.

## Key terms
- **MAR / MBR**: address and data buffer registers for memory access.
- **PC / IR / PSW**: next-instruction address / current instruction / status and mode bits.
- **Word size**: size of a CPU register; matches the data bus width.
- **Interrupt**: signal that suspends normal instruction sequencing.
- **Interrupt handler**: OS routine servicing an interrupt.
- **Memory hierarchy**: levels trading speed against cost and capacity.
- **Locality of reference**: accesses cluster in time and space, so a small fast memory is effective.
- **Cache hit / miss**: needed word is / is not in cache.
- **Disk cache**: main-memory buffer for disk data.
- **Programmed I/O, interrupt-driven I/O, DMA**: the three ways of doing I/O, in increasing order of CPU relief.

## Questions
[Practice questions](../questions/topic-01-questions.md)

## Sources
- Slides: ICT374Topic01.pdf, slides 1-49 (slides 2-5 objectives and readings; 6-8 basic elements; 9-14 registers; 15-18 instruction cycle; 19-34 interrupts; 35 multiprogramming; 36-45 memory and cache; 46-49 I/O).
- Stallings, Chapter 1: 1.1 Basic Elements (p. 30), 1.3 Instruction Execution (p. 32), 1.4 Interrupts (p. 35), 1.5 The Memory Hierarchy (p. 46), 1.6 Cache Memory (p. 49), 1.7 Direct Memory Access (p. 53).
- Note: slide figures could not be rendered in this environment (no pdftoppm); figure descriptions (Fig 1.1, 1.5, 1.10, 1.11, 1.16-1.18) follow the slide captions and Stallings. The step list in "Simple interrupt processing" is from Stallings Fig 1.10, p. 41.
