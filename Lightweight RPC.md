# [Lightweight remote procedure calls](http://www.cs.virginia.edu/~zaher/classes/CS656/bershad.pdf)

## Main ideas
* Reducing overhead of RPC calls in the same host (cross-domain)
* Assumption that most calls are cross-domain and not cross-machine

## 4 key techniques
* Client thread runs procedure in server's domain
* Client and server share access to the A-stack (argument stack)
* Simpler stubs (mechanisms to make server believe arguments are on its address space)
* Avoidance of shared data structures to improve multiprocessor performance

## Overview of regular RPC for comparison
* RPC of a null procedure consists of at least 3 context switches:
    1. Procedure call
    2. Kernel trap and change of processor's virtual memory
    3. Trap again to send procedure to server
    4. Return value from server requires another context switch
* Stubs in RPC are general (usable in cross-machine RPC) and therefore expensive to create
* Overhead of copying arguments 4 times:
    1. client to kernel
    2. kernel to server
    3. server to kernel
    4. kernel to client
* Validation of message sender - the kernel cannot just trust any RPC trap
* Two context switches

## Implementation of LRPC
* Execution-stack creation is delayed until needed
* E-stack <-> A-Stack association delayed until needed
* This makes the call faster

### Binding
* Same as RPC, servers export interfaces, clients bind to them
* Implementation:
  * Clerk registers the interface with the server
  * Client bind to said interface by making import call via kernel
  * **Kernel allocation begins**
  * Clerk replies to kernel with a Procedure Descriptor List which contains:
    * a descriptor for every procedure in the interface
    * the size of the A-stack for arguments
  * Kernel allocates an A-Stack for each Procedure Descriptor
  * Kernel allocates a "linkage record" for each A-Stack which links caller return address with A-Stack
  * **Kernel allocation ends**
  * Client receives a Binding Object (BO) and A-Stack list.

### Calling
* Procedures are represented by a stub in the client and server
* Stub is responsible for pushing client arguments into A-Stack and trapping to kernel
* Kernel validates trap -> finds available E-Stack (more on it later) in server domain
* Kernel uploads client's thread stack pointera to run off the E-Stack
* Kernel performs upcall into server stuff at Procedure Descriptor found in the BO
* Kernel procedure returns value through its own stub

## Multiprocessors
* Cache domain context on idle processors
* On every procedure, the kernel can look for processors with warm cache
* If a warm cache is found it:
  * avoids context switching
  * preserves locality between RPC calls!

## Argument copying
* In traditional RPC - 4 copies
  * Stub stack to RPC message
  * Message in client domain to kernel
  * Kernel to server domain
  * Server domain to server stack
* In LRPC - 1 copy:
  * Client stub stack to shared A-stack
* Much lighter and w/o context switching


