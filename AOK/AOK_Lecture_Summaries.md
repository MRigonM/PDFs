# AOK - Lecture Summaries

> Lectures 1, 2, 3 are in old binary .ppt format and could not be read automatically.
> Summaries below cover Lectures 2, 3, 4, 5, and 6 based on the available PDFs.

---

# Lecture 2 - Computer Evolution and Performance (Evolucioni i Kompjuterit dhe Performanca)

## Early Computers

The first general-purpose electronic computer was **ENIAC** (1943–1946), built at the University of Pennsylvania. It used decimal arithmetic, 18,000 vacuum tubes, weighed 30 tons, consumed 140 kW of power, and could perform 5,000 additions per second. It was programmed manually via switches.

The breakthrough came with the **von Neumann / stored program concept** (IAS machine, completed 1952): programs and data both live in main memory, a binary ALU processes data, and a control unit fetches and executes instructions. This architecture is the foundation of all modern computers.

## Generations of Computer Hardware

| Generation | Technology | Years |
|---|---|---|
| 1st | Vacuum tubes | 1946–1957 |
| 2nd | Transistors | 1958–1964 |
| 3rd | Small-scale integration (up to 100 devices/chip) | 1965+ |
| 4th | Medium-scale integration (100–3,000) | to 1971 |
| 5th | Large-scale integration (3,000–100,000) | 1971–1977 |
| 6th | VLSI (100,000–100M) | 1978–1991 |
| 7th | ULSI (100M+) | 1991– |

**Transistors** replaced vacuum tubes: smaller, cheaper, less heat. Invented 1947 at Bell Labs. **Microelectronics** allowed gates, memory cells, and interconnections to be manufactured on a silicon wafer.

## Moore's Law

Gordon Moore (Intel co-founder) observed that transistor count on a chip doubles roughly every 18 months, with chip cost staying nearly constant. Higher packing density → shorter electrical paths → higher performance, smaller size, lower power.

## Key Milestones

- **IBM 701** (1953): IBM's first stored-program computer.
- **IBM 360** (1964): First planned "family" of compatible computers — same instruction set, same OS, scaling in speed, memory, and I/O.
- **DEC PDP-8** (1964): First minicomputer. Introduced the **bus structure** (Omnibus) — all components on a single shared pathway.
- **Intel 4004** (1971): First microprocessor — entire CPU on one chip, 4-bit. Followed by 8008 (8-bit) and the 8080 (1974), Intel's first general-purpose microprocessor.

## x86 Evolution

8080 → 8086 (16-bit, instruction prefetch) → 80286 → 80386 (32-bit, multitasking) → 80486 (pipeline + FPU) → Pentium (superscalar) → Pentium Pro (out-of-order execution, branch prediction) → Pentium II/III/4 (MMX, multimedia, FP extensions) → Core (first dual-core x86) → Core 2 (64-bit) → Core 2 Quad (4 cores, 820M transistors).

## Performance Balance Problem

Processor speed has grown much faster than memory speed. The CPU-memory gap is a central challenge. Solutions include:
- Wider DRAM (more bits retrieved at once)
- On-chip cache (reduce memory access frequency)
- High-speed buses and bus hierarchies
- Caching and buffering for I/O

## Improving Processor Performance

Three approaches: (1) increase hardware clock speed (shrinking gate size), (2) increase cache size and speed (Pentium 4 devotes ~50% of chip area to cache), (3) improve organization — pipelining, superscalar execution, branch prediction, speculative execution.

Limits exist: power density grows with clock speed, RC delay increases as wires shrink, memory latency remains a bottleneck. The solution is **multiple cores**: doubling processors (if software is parallel) nearly doubles performance, whereas doubling single-core complexity gives only ~√2 improvement.

## Performance Metrics

**Clock speed** (Hz) is not the whole story — instructions take multiple cycles, and pipelining allows overlap.

**MIPS** (millions of instructions per second) and **MFLOPS** (floating point) are common metrics but depend heavily on the instruction set, compiler, and memory hierarchy.

**SPEC benchmarks** (System Performance Evaluation Corporation) are standardized programs used to compare systems. CPU2006 uses 17 FP and 12 integer programs. Results use geometric mean of ratios (system time vs. reference machine time).

## Amdahl's Law

Limits the speedup from parallel processors. If fraction *f* of code is parallelizable and fraction *(1-f)* is serial, with *N* processors:

> **Speedup = 1 / [(1 - f) + f/N]**

As N → ∞, speedup is bounded by **1/(1 - f)**. If little code is parallelizable, adding processors helps very little. Diminishing returns apply.

---

# Lecture 3 - Top Level View of Computer Function and Interconnection (Pamja e Nivelit të Lartë)

## The Program Concept

Hardwired systems are inflexible. General-purpose hardware achieves flexibility through **software**: instead of rewiring, supply new control signals (instructions). A program is a sequence of steps; each step specifies an arithmetic or logical operation; each operation requires a different set of control signals issued by the Control Unit.

## Computer Components

The CPU consists of the **ALU** (Arithmetic-Logic Unit) and the **Control Unit**. Together with **main memory** and **I/O modules**, connected via a **system bus**, these form the complete machine.

Key CPU registers: PC (Program Counter), IR (Instruction Register), MAR (Memory Address Register), MBR (Memory Buffer Register), I/O AR, I/O BR.

## Instruction Cycle

Every instruction goes through two phases:

1. **Fetch**: PC → MAR, read memory → MBR → IR, increment PC.
2. **Execute**: decode IR, perform the operation (data transfer, arithmetic, I/O, control/jump).

The full state sequence: Instruction address calculation → Instruction fetch → Instruction operation decoding → Operand address calculation → Operand fetch → Data operation → Operand store → (repeat).

## Interrupts

Interrupts let external modules (I/O, timers, hardware errors) break into normal execution. Types:
- **Program**: overflow, division by zero
- **Timer**: generated internally, used for pre-emptive multitasking
- **I/O**: from I/O controller when operation completes
- **Hardware failure**: e.g. memory parity error

**Interrupt cycle** (added after execute): check for interrupt signal → if pending: save context (PC + registers), load interrupt handler address into PC → execute handler → restore context → resume program.

**Multiple interrupts** are handled either by disabling further interrupts during handling (sequential, simpler), or by priority levels where a higher-priority interrupt can itself interrupt a lower-priority handler (nested).

## Buses

A **bus** is a shared communication pathway connecting two or more devices. It broadcasts — all connected devices see the signal, but only the addressed one responds. A bus typically has three groups of lines:

| Bus | Purpose |
|---|---|
| Data bus | Carries data (and instructions — no difference at hardware level). Width (8/16/32/64 bit) determines performance. |
| Address bus | Identifies source or destination (memory location or I/O port). Width determines max addressable memory. |
| Control bus | Carries timing and control signals: read/write, interrupt request, clock. |

**Single bus problems**: many devices on one bus cause propagation delays and contention. Real systems use **multiple buses** in a hierarchy (local bus → system bus → expansion bus).

## Bus Arbitration

When multiple masters exist (CPU + DMA controller), only one may drive the bus at a time. Arbitration is either:
- **Centralised**: a single bus controller/arbiter grants access.
- **Distributed**: each module has control logic; they negotiate among themselves.

## Bus Types

- **Dedicated**: separate physical lines for data and address.
- **Multiplexed**: data and address share lines (fewer wires, but more complex control and lower peak performance).

---

# Lecture 4 - Cache Memory (Memoria Cache)

## Memory Hierarchy

Computer memory is organized as a hierarchy. From top to bottom: CPU registers, Cache, Main Memory (DRAM), then external storage (disks, optical, tape). Going down the hierarchy: price per bit drops, capacity increases, and access time gets slower.

## Memory Characteristics

Key properties used to classify memory:
- **Location**: CPU (registers), internal (cache/RAM), external (secondary)
- **Capacity**: word size for internal, byte count for external
- **Access methods**: Sequential (tape), Direct (disk), Random (RAM), Associative (cache)
- **Performance**: access time, cycle time, transfer rate
- **Physical type**: semiconductor, magnetic, optical

## What is Cache

Cache is a small, fast memory placed between the CPU and main memory. It stores recently or frequently used data. When CPU requests data, it checks cache first. If found (hit), it's fast. If not found (miss), the block is loaded from main memory into cache.

## Cache Design Elements

Six key decisions when designing a cache:
- **Size**: more cache = faster, but more expensive
- **Mapping function**: how memory blocks map to cache lines
- **Replacement algorithm**: which block to evict on a miss
- **Write policy**: how writes are handled
- **Block/line size**: 8-32 bytes is generally optimal
- **Number of caches**: L1/L2/L3, unified or split

## Mapping Functions

**Direct Mapping**: each main memory block maps to exactly one cache line. Simple and cheap, but causes thrashing if two frequently used blocks share the same line.

**Associative Mapping**: a block can go into any cache line. Very flexible but expensive — requires checking all tags simultaneously.

**Set Associative Mapping**: cache is divided into sets, each set has k lines. A block maps to any line within a specific set. Best compromise between direct and associative.

## Replacement Algorithms (for associative/set-associative)

- **LRU** (Least Recently Used): replace the block unused for the longest time — most common
- **FIFO**: replace the oldest block in cache
- **LFU** (Least Frequently Used): replace the block with fewest accesses
- **Random**: simple but unpredictable

## Write Policies

**Write Through**: all writes go to both cache and main memory simultaneously. Consistent but slow and high traffic.

**Write Back**: writes go only to cache first; main memory is updated only when the block is evicted. Faster, but other caches or DMA may see stale data.

## Multiple Cache Levels

Modern processors use L1 (on-chip, fastest), L2 (on or off chip), and sometimes L3. L1 is often split into separate instruction and data caches to avoid contention. Pentium 4 example: L1 8KB, L2 256KB, L3 on-chip.

---

# Lecture 5 - Internal Memory (Memoria e Brendshme)

## Semiconductor Memory Types

| Type | Category | Erasable | Volatile |
|------|----------|----------|----------|
| RAM | Read-write | Electrically (byte) | Yes |
| ROM | Read-only | No | No |
| PROM | Read-only | No | No |
| EPROM | Read-mostly | UV light (chip) | No |
| EEPROM | Read-mostly | Electrically (byte) | No |
| Flash | Read-mostly | Electrically (block) | No |

## DRAM (Dynamic RAM)

Bits stored as charge on capacitors. Capacitors discharge over time, so periodic refresh is required. Simple construction, smaller per bit, cheaper. Used for main memory. Inherently analog — charge level determines value.

## SRAM (Static RAM)

Bits stored using flip-flop logic (like in a processor). No refresh needed. More complex, larger per bit, more expensive. Much faster than DRAM. Used for cache.

## SRAM vs DRAM Summary

DRAM: simple, cheap, dense, needs refresh, slower → main memory.
SRAM: fast, no refresh, expensive → cache.

Both are volatile — power is required to retain data.

## ROM Types

ROM stores permanent data (BIOS, firmware, function tables). It is nonvolatile.
- **ROM**: written during manufacturing, cannot be changed
- **PROM**: user-programmable once with special equipment
- **EPROM**: erased with UV light at chip level, reprogrammable
- **EEPROM**: erased electrically byte-by-byte; writing much slower than reading
- **Flash**: erased electrically at block level — compromise between EPROM and EEPROM

## Error Correction

Two types of errors:
- **Hard errors**: permanent hardware defects
- **Soft errors**: random, non-destructive, no permanent damage

Detected and corrected using **Hamming error-correcting code**. Extra check bits are stored alongside data. On read, the code is recalculated and compared to detect and correct single-bit errors.

## Advanced DRAM Organizations

**SDRAM (Synchronous DRAM)**: access synchronized with external clock. CPU knows exactly when data will be ready and can do other work while waiting. Supports burst mode for block transfers.

**DDR-SDRAM**: sends data twice per clock cycle — on both rising and falling edge.

**Cache DRAM (CDRAM)**: integrates a small SRAM cache (16KB) inside the DRAM chip. Effective for both random access and serial block access.

**RAMBUS DRAM**: high-speed bus (1.6 Gbps), used in Pentium and Itanium. Transfers data over 28 lines with asynchronous block protocol.

---

# Lecture 6 - External Memory (Memoria e Jashtme)

## Types of External Memory

- Magnetic disks (hard disks, floppy)
- Optical (CD-ROM, CD-R, CD-RW, DVD)
- Magnetic tape

## Magnetic Disk

A disk substrate (originally aluminum, now glass) coated with magnetizable material (iron oxide). A read/write head moves over spinning platters. The head does not move while reading/writing — the platter rotates under it.

Writing: electric current through the head coil creates a magnetic field, recorded on the surface.
Reading (modern): a separate magnetoresistive read head detects field direction changes. Resistance varies with field direction.

## Disk Data Organization

Data is arranged in concentric tracks, each divided into sectors. Minimum block size is one sector. Tracks at the outer edge waste space with constant angular velocity — zone bit recording fixes this by using more bits per outer track.

## Disk Performance

- **Seek time**: time to move the head to the correct track
- **Rotational delay**: waiting for the right sector to rotate under the head
- **Access time** = seek time + rotational delay
- **Transfer rate**: speed at which data is read once head is in position

## Multiple Platters and Cylinders

Hard disks have multiple platters, each with two read/write surfaces. Tracks at the same radial position across all platters form a cylinder. Reading from the same cylinder avoids head movement and improves speed.

## RAID (Redundant Array of Independent Disks)

Disk access is much slower than CPU/RAM. RAID uses multiple disks in parallel to improve speed and/or reliability. There are 7 levels (0–6), not hierarchical.

- **RAID 0**: striping only, no redundancy. Maximum performance, no fault tolerance.
- **RAID 1**: mirroring. Two copies of all data. Easy recovery, expensive.
- **RAID 2**: Hamming code across disks. Very redundant, not used in practice.
- **RAID 3**: one dedicated parity disk. Good for high transfer rates.
- **RAID 4**: block-level parity on one disk. Good for large I/O requests.
- **RAID 5**: like RAID 4 but parity distributed across all disks. Used in network servers.
- **RAID 6**: two parity calculations on separate disks. Three disks must fail before data is lost. Most fault-tolerant but writes are expensive.

## CD-ROM

Polycarbonate disc coated with aluminum. Data stored as pits and lands. Read by laser — pit/land transitions represent bits. Capacity ~650MB (~70 minutes audio). Read-only, constant linear velocity, good for mass distribution but slow and expensive for small runs.

## Other Optical Media

- **CD-R**: writable once (WORM — Write Once Read Many). Compatible with CD-ROM drives.
- **CD-RW**: erasable and rewritable using phase-change material with two reflectivity levels.
- **DVD**: multi-layer, 4.7GB per layer, up to 17GB for double-sided dual-layer. Supports MPEG compression for full-length films. Writable variants have standardization issues.

## Magnetic Tape

Sequential (serial) access only — no random access. Very slow. Very cheap. Used for backup and long-term archival storage.