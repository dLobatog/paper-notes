#[Memory resource management in VMware ESX server](http://www.waldspurger.org/carl/papers/esx-mem-osdi02.pdf)

## Key ideas
* Ballooning to allow memory overcommittment over all VMs
* Idle memory taxing
* Content-based page sharing
* I/O page remapping

## Memory virtualization
* VM has the illusion that "physical addresses" are zero based
* pmap data structure maps "physical page" number with machine page number
  * VM instructions that would modify memory are intercepted and remapped
* Shadow page tables map virtual pages to machine pages to avoid 1 level of indirection ("physical pages")

## Page replacement
* When memory is overcomitted, host has to reclaim space
* Problem: OS in VM may reclaim pages that were just paged out

## Ballooning
* Balloon module in guest OS allocates "physical pages"
* When inflated - OS realizes how much memory it has available (balloon size)
* When deflated - OS allocates balloon pages to more sensible uses
* If balloon driver is unavailable, ESX will swap in disk

## Sharing memory
* If VMs run the same OS or same application, share the actual machine pages
* pmap of each VM makes the mapping to the same machine page

## Transparent page shareing
* Once copies are identified, map "physical pages" to machine pages
* Disco's approach, ESX improves it with...

## Content-based page sharing
* Hashing to match memory pages between all domains
* If page contents are found in hash table, success
  * Make machine page shared
* If page contents are not found in hash table
  * Mark it as a hint entry, in case of future matches

## Implementation
* Global hash table for all scanned pages
* Shared frame: hash value, MPN, references count
* Hint frame: hash value, MPN, PPN, VM identifier
* Pages are scanned randomly to check equality

## Share-based allocation
* Resource rights are determined by shares, owned by clients
* Memory is revoked based on whether client owns enough shares per page

## Idle memory tax
* Clients are charged more for idle pages than used ones
* Ensures memory that is idle is reclaimed first
* Host does statistical sampling of pages to measure idleness
* A small buffer of 25% of idle pages is left in VM to allow for rapid mem increases

## I/O page remapping
* ESX keeps track of "hot" pages that are frequently used for I/O in the guests
* Since I/O happens on low memory normally, ESX maps those "physical pages" to low memory
* \* low memory is memory permanently mapped into kernel address space
