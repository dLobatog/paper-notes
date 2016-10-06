#[Using processor-cache affinity in shared memory multiprocessor scheduling](https://eds-a-ebscohost-com.prx.library.gatech.edu/eds/detail/detail?sid=0295c83e-0e22-463f-9664-dbe19ed1f13d%40sessionmgr4008&vid=0&hid=4205&bdata=JnNpdGU9ZWRzLWxpdmUmc2NvcGU9c2l0ZQ%3d%3d#AN=edselc.2-52.0-0027541973&db=edselc)

## Key ideas
* Take advantage of processor cache to schedule tasks
* Investigate how affinity affects schedulers

## Forms of affinity
* How fast the task runs given heterogeneous processors (uncommon)
* How the processor cache affects processor performance
* Processor may have resources (I/O..) associated with it

## Cache reload transient
* The burst of cache misses that happens on the first run of a task

## Cache footprint
* Group of cache blocks used by task
* Measurements in this paper do not take into account false data sharing

## The policies examined
* First come first served
* Fixed processor
* Last processor
  * The last processor that ran a task is where it'll run the next time
* Minimum intervening
  * Before scheduling, compute the number of tasks waiting to run in all processors
  * Task will be scheduled to the processor with the minimum number of waiting tasks
* Limited minimum intervening
  * Same as MI but limiting the number of tasks a processor can have affinity with
* LMI routing
  * Same as LMI, but the scheduling decision happens when task becomes
    schedulable, not when processor is idle
  * For each processor, compute the sum of all tasks in queue + sum of intervening
    tasks
  * Task gets scheduled for the minimum of the measurement above

## Comparison and discussion (largely skipped)
* LMIR very similar to FP - which provides usually the best throughput
* However significant differences between LMIR and other MI policies
only show up when the number of tasks is very high.
