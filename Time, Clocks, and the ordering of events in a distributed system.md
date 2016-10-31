# [Lamport clocks](http://research.microsoft.com/en-us/um/people/lamport/pubs/time-clocks.pdf)

## Main points

* Each node knows about its own events and its communication events
* Clocks work under two conditions
  * Monotonic increase of it's own event times (e.g A happens before B, locally)
  * Message receipt time is always greater than send time
* Partial order (given C(a) < C(b) we cannot infer a happened before b)

## Concurrency

* If all nodes have the same timestamp to request something, the oldest node wins
* Other arbitrary conditions would work too

## Distributed lock M.E algorithm

* Every process keeps a queue
* State of the queue is the same for all processes
* The process know if it has the lock if:
    * Its request is at the top of the queue
    * Received acks from all the nodes in the systems
* To unlock, process sends an unlock message (remove from queue)
* In total, it sends:
  * N - 1 messages to acquire the lock (request),
  * N - 1 messages to ack i
  * N - 1 to release

### Correctness

* Some assumptions:
  * Messages arrive in order
  * No message loss
  * Queues are totally order by Lamport Clock by PID to break ties

### Improvements

* Defer ACKs if 'my req' precedes 'your req' - combine with the unlock message

## Problems of Lamport clocks

* They need small individual and mutual clock drift to work well.
* Individual clock drift and mutual clock drift must be smaller than
inter process communication time
