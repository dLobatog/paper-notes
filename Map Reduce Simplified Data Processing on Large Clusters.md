#[MapReduce: Simplified Data Processing on Large Clusters](https://static.googleusercontent.com/media/research.google.com/en//archive/mapreduce-osdi04.pdf)

## Key ideas
* Map processes K,V pairs into intermediate K,V pairs
* Reduce merges all V associated with a K
* Embarrassing parallelism on commodity hardware
* Inspired by LISP

## Types
* map(K1, v1) -> list(K2, v2)
* reduce(K2, list(v2)) -> list(v2)

## Implementation
* Google's sample in 2004:
* Input data is partitioned into M splits
* Reduce invocations partitioned by using hash functions over the intermediate space
* First copy of Map is the master.
  * Subsequent copies are workers which get one of the M splits assigned
  * When they finish, they report to master with the location of the intermediate data
* Workers get instructed by master to pick up intermediate data via RPC calls

## Fault tolerance
* worker failure: timeout set by master. task reassigned if timeout
* master failure: master makes checkpoints of its internal data structure that a new master can pick up

## Locality
* Master knows where the data is stored via GFS
* Master assigns tasks to workers most likely to have a replica of the data the task needs

## Backups
* When a MapReduce operation is straggled, master backs up all of the in-progress tasks.

## Ordering guarantees
* Within a given partition, the intermediate K,V pairs are sorted
* Sorted output of reducer per partition
* Random access to output is easier this way

## Combiner function
* Sometimes the intermediate keys are the same
* For example: counting words in a sentence will produce a lot of <'the', 1> pairs
* An user can specify a combiner function to merge some data before sending it to the network

## Input and output types
* Text: every line is a K,V pair
* Users can a reading interface to Map or Reduce that implements new types

## Skipping bad records
* If the map or reducer find they cannot process a record deterministically, those can be skipped

## Status information
* Master shows via HTTP how many pending tasks there are, bitrate, processing rates, status, etc..

## Counter
* Users can provide a global counter object which periodically propagates results to master
* Helpful for benchmarking map or reduce tasks
* Helpful for sanity checks - e.g: counter for subtask X should be the same in Map and Reduce.
