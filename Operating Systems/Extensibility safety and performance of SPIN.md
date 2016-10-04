#[Extensibility safety and performance of SPIN](http://cseweb.ucsd.edu/~savage/papers/Sosp95.pdf)

## Key idea: Extensible OS
* Applications make special calls to the OS to extend and specialize its services
* Protection of domains is ensured via Modula 3 typed-pointers
* example: database app might want to bring its own disk buffering system, word processor might
be OK with the default provided one

## Goals
* Extensibility, safety and good performance

## Approach
* **Colocation**
  * Dynamic link of kernel extensions into kernel address space
* **Enforced modularity**
  * Modula-3 etensions run in kernel address space but cannot run privileged instructions or access privileged objects
* **Logical protection domains**
  * Kernel namespaces which contain critical code and interfaces, protected at compile time via typed pointers
* **Dynamic call bindings**
  * Extensions declare how to handle events like page faults, tcp ack received, etc...

## Capabilities
* Unforgeable reference to kernel resources such as physical pages or system objects
* Referenced via object pointers
* Protection at compile time of these pointers

## Protection domains
* Accessible names within execution context
* 3 instructions to interact with them:
  * Create: initiate object file and export names
  * Resolve(name, domain): resolve unresolved names in current execution context (dynamic linking)
  * Combine: aggregate logical protection domains into one

## Extensions
* Defined in terms of events and handlers
* Kernel provides central dispatcher for these events
* e.g of event/handler: IP.packetArrived -> Name.inspectHeaders

## Core service: memory management
* Allocation of physical, virtual pages and mapping between them

## Core service: thread management
* 'Strands' interface for applications that want to provide their thread scheduler
  * Strands are an abstraction over threads that provides context of CPU to thread extension authors
* Event handlers:
  * block, unblock, checkpoint, resume
  * calls to the above are used at the application level to provide your own scheduler (extensibility)

## Performance - skipped
