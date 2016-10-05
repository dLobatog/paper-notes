#[Exokernel: An Operating System Architecture for Application-Level Resource Management](https://pdos.csail.mit.edu/6.828/2008/readings/engler95exokernel.pdf)

## Key ideas
* Small kernel exports hardware resources to 'library' OSs
* Allow domain-specific optimizations
* Application writers select their own OS libraries or write their own

## Motivation
* Overly general abstractions are not performant in monolithic kernels
* Applications know more than the kernel about how to manage resources for themselves

## Design principles
* Avoid resource management by the kernel
* Expose physical allocation
* Expose physical naming
* Make resources revocation public

## Secure bindings
* Authorization over a resource - it's decouple from management
* Implemented via:
  * Hardware mechanisms
  * Software caching
  * Download code into the kernel

## Multiplexing physical memory
* Library OS allocates physical page and receives capability
  * Secure binding is created, including owner, read/write permissions
  * Before granting future accesses, kernel requires capability
* Hardware TLB loads or DMA still guarded by kernel
* Library OS presents capability as "access card" for each resource

## Multiplexing network
* Packet filtering code is downloaded from library OS to kernel
* Securing problem: packet filter can 'lie' and accept packets not intended for library

## Downloading code to kernel
* Elimination of library-kernel border crossings
* Allows for handling things at high speed that apps cannot do because border cossings are slow
* e.g: application handlers for packets which respond to packets faster than applications

## Visible resource revocation
* Exokernel reveals revocation into library OS
* Physical naming references in library OS are then updated or relinquished
* e.g: Remove a physical page, remap references in OS

## Abort protocol
* Secure bindings can be force broken by library OS
* Library OS receives a repossession vector (as an exception)
  * Repossession vector can be used to update the mapping of resource in library OS
* Library OS can ask for a small set of resources, say 5-10 physical pages, and mark them
as vital, which can never be relinquished
