#[On microkernel construction](http://read.seas.harvard.edu/cs261/papers/liedtke95on.pdf)

## Key idea
* Refute that microkernels are inefficient and inflexible

## Rationale for microkernel
* As little code in kernel space as possible
* Modular system design
* Errors in programs are isolated to userspace

## Address spaces
* Microkernel hides the TLB and hardware page tables to protect userspace
* Address spaces are constructed in a recursive, stack fashion:
  * σ0 represents the HW address space, managed by S0
  * σn > 0 represents any other address space, managed by Sn and on top of Sn-1
* **Grant**:
  * owner of address space can grant pages it owns
  * removes them from current address space
* **Map**:
  * Owner can map its pages into other address space
  * Pages are accessible by 2 address spaces
* **Flush**:
  * Owner can remove page from everywhere but flushers' address space
* I/O device ports can be represented as address spaces

## Threads and IPC
* Threads can be dynamic or static associated to an address space
* Microkernel governs changes of thread address space, to prevent memory corruption
* IPC supported by microkernel, only messaging
* Hardware interrupts abstracted as IPC messages
  * microkernel does not handle the interrupts, just sends the IPC message to threads
* Microkernel grants UID to threads

## Flexibility
* Memory managers can be build in a stacked fashion
* Pagers can be defined at user-level and be implemented via just Grant, Map, Flush calls
* Device drivers can be written at user level to handle I/O IPC interrupt messages

## Non-portability
* Unlike L3 , Mach was built for abstract hardware
* Bad performance when building kernels for portability, as they cannot ctake advantage of specific HW
