# [Lightweight Recoverable Virtual Memory](http://dl.acm.org/citation.cfm?id=168631&dl=ACM&coll=DL&CFID=832963580&CFTOKEN=77678292)

## Key ideas
* Simple transactions used for fault tolerance
* Low system resources footprint
* Good learning curve

## Lessons from Camelot (predecessor)
* Relies on external page management and Mach IPC
* Two-phase optimistic replication protocol in CodaFS is what peeked their interest
* lesson: data structures in disk improve crash recovery
          simplify CodaFS code as crash recovery was no longer in the same source
* problems: all Coda processes had to be children of a disk manager process
            this is hard to debug when Coda is under that condition
            not scalable due to high cpu usage
* Camelot was a research experiment, not a production project

## Design rationale
* Simplicity > generality
* Eliminate support for nesting and distribution of transactions
* Keep metadata of recoverable VM in system
* Factor out concurrency control and parallelism
* Factor out resiliency to media failure
* External data segment - backing storage of memory

## Structure
* IPC is 600x slower than local procedure calls
* Thus, LRVM will be linked with your program code
* In a log-structured UNIX file system - placing one RVM log per app would be very performant

## Architecture
* Recoverable memory managed in segments up to 2^64 B long
* Backed by a disk or file
* Segment regions typically represent collections of objects
* Copy to segment happens when memory is mapped

## RVM primitives
* initialize(version, options)
* map(region, options)
* unmap(region)
* begin_transaction(tid, restore_mode)
* set_range(tid, base_addr, nbytes)
  * Let's RVM know that an area of the region will be modified
* end_transaction(tid)
* abort_transaction(tid)
* To ensure persistence of no-flush transaction LRVM must flush writeahead log from time to time
* flush() blocks until all committed no-flush transactions have been committed to disk
* truncate() blocks until all committed changes in writeahead log have been written to segment
* other: query(), set_options(), create_log()

## Implementation - log management
* log format: never reflect uncommitted changes to external memory
* set_range of old value records
  * upon commit, range is replaced by new records
* no-restore, no flush transaction are more efficient
* log can be read backward and forward

## Implementation - crash recovery and log truncation
* RVM reads log from tail to head
* Creates in memory tree of latest committed changes
* Tree is traversed and modifications are made to the external data segment
* Finally - mark content between head and tail as empty
* Periodic truncation reclaims space assigned to these logs
* Sometimes truncation can happen while concurrently it's being processed
* Incremental truncation: write out relevant pages from VM to oldest log entries
  * This renders logs obsolete
  * Ensure pages modified by uncommitted transactions are not written to

## Optimizations
* Intra-transaction: when overlapping memory addresses within a transactoin are mapped using set_range
  * duplicate set_range calls: ignored
  * adjacent set_range calls: coalesced
* Inter-transaction: when same transactions is performed repeatedly, only flush the last one
  * e.g: 'cp * c2' could flush c2 multiple times w/o optimization

## Status
* Over 2 years of experience: successful on commodity hardware
* Unexpected use: debugging CodaFS crashes by examining the logs
* Benchmarks w/Camelot to compare how much we lose by not including RVM on the OS
  * nearly identical throughput, RVM usually faster and performant
  * Camelot disk manager to blame about higher system resource usage
* Savings of 20-30% with each of inter/intra transaction optimizations

## Conclusion
* Useful to maintain persistent data structures with clean failure
* Minimal impact on system resources usage  and low learning curve



