# OS-Pintos-Project
This repository contains the Pintos kernel code, focusing on implementing core OS functionalities across user programs, file systems, and thread scheduling.

**Current Status:** Project \#1, \#2, and \#3 implementations are complete and uploaded. Project \#4 is currently in progress.

-----

## 📂 Repository Structure

The repository maintains a structured layout, separating the components for each project:

```
OS-Pintos-Project/
├── Project1/
│   ├── 20222089.docx      // Project Report
│   ├── 2025_os_proj1.pdf  // Project Specification
│   └── os_prj1_20222089.tar.gz // Compressed Source Code (src/)
├── Project2/
│   ├── 20222089.docx      // Project Report
│   ├── 2025_os_proj2.pdf  // Project Specification
│   └── os_prj2_20222089.tar.gz // Compressed Source Code (src/)
├── Project3/
│   ├── 20222089.docx      // Project Report
│   ├── 2025_os_proj3.pdf  // Project Specification
│   └── os_prj3_20222089.tar.gz // Compressed Source Code (src/)
└── README.md
```

-----

## 🚀 Project Summary and Implementation Overview

### 1\. Project \#1: User Program (I)

This project focused on enabling the Pintos kernel to properly execute user programs by implementing fundamental execution mechanisms and basic system calls.

  * [cite\_start]**Goal:** Implement core functionalities for executing ELF (Executable & Linkable Format) user programs[cite: 589].
  * [cite\_start]**Argument Passing:** Implemented parsing and allocating command-line arguments to the user stack according to the $80\times86$ calling convention, starting from the top of `PHYS_BASE`[cite: 1175, 1226].
  * [cite\_start]**System Calls:** Implemented the system call handler and general system calls required for process control: `halt`, `exit`, `exec`, `wait`, `read`, and `write`[cite: 1325].
      * [cite\_start]**STDIO:** `read` and `write` were implemented to handle standard input (`STDIN=0`) using `input_getc()` [cite: 1357, 1359] [cite\_start]and standard output (`STDOUT=1`) using `putbuf()`[cite: 1357, 1361].
  * [cite\_start]**Termination Message:** Implemented printing the process termination message upon exit, in the required format: `Process Name: exit(exit status)`[cite: 1112].

-----

### 2\. Project \#2: User Program (II) - File System

This project extended the system call functionality to support file system operations, allowing user programs to manage files on the Pintos file system.

  * [cite\_start]**Goal:** Implement file system related system calls (`create`, `remove`, `open`, `close`, `filesize`, `read`, `write`, `seek`, `tell`)[cite: 21].
  * [cite\_start]**File Descriptors:** Each thread manages an independent set of file descriptors (FDs)[cite: 140].
  * [cite\_start]**Synchronization:** Implemented synchronization mechanisms (Locks) within file system system calls (`syscall_handler`) to protect critical sections and shared file data, addressing concurrency issues[cite: 198, 204]. [cite\_start]This is essential for preventing races during file access (e.g., `syn-read`, `syn-write` tests)[cite: 199].
  * [cite\_start]**Executable Protection:** Used `file_deny_write()` and `file_allow_write()` APIs to prevent the running executable file from being deleted or modified while it is active in memory[cite: 186, 187].

-----

### 3\. Project \#3: Threads

This project focused on enhancing the kernel's scheduler from the default **Round-Robin** to a more complex and efficient priority-based scheduler.

  * [cite\_start]**Alarm Clock:** Modified `timer_sleep()` to block the thread (`THREAD_BLOCKED`) instead of using inefficient busy-waiting (RUNNING ↔ READY loop)[cite: 2086, 2094]. [cite\_start]Threads are managed in a separate sleep queue and are awakened by `timer_interrupt()`[cite: 2098].
  * **Priority Scheduling:**
      * The **Ready List** is maintained in priority descending order using `list_insert_ordered`.
      * [cite\_start]Implemented **Preemption**: If a newly ready thread has a higher priority than the running thread, the running thread immediately yields the CPU[cite: 2125].
      * [cite\_start]**Aging Technique:** Implemented Aging to gradually boost the priority of low-priority threads residing in the ready list, thus preventing starvation[cite: 2180, 2181].
  * [cite\_start]**Advanced Scheduler (BSD/MLFQS):** (Optional/Additional Implementation)[cite: 2263].
      * [cite\_start]**Fixed-Point Arithmetic:** Implemented fixed-point real arithmetic (p.q format, 17.14) to handle real numbers like `load_avg` and `recent_cpu` in the kernel, as floating-point is unsupported[cite: 2396, 2398].
      * [cite\_start]**Dynamic Priority:** Recalculated priorities every 4 ticks based on the formula: $priority=PRI\_MAX-(recent\_cpu/4)-(nice^{*}2)$[cite: 2333, 2334, 2336].
      * [cite\_start]**Load Average:** System-wide `load_avg` is updated every second based on the number of ready/running threads: $load\_avg=(59/60)*load\_avg+(1/60)*ready\_threads$[cite: 2368].

-----

## 🛠️ Development Environment

  * **OS:** Pintos (Custom Kernel)
  * **Environment:** QEMU on Linux (Ubuntu)
  * **Language:** C
