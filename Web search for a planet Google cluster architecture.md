#[Web search for a planet: Google cluster architecture](http://static.googleusercontent.com/external_content/untrusted_dlcp/research.google.com/en/us/archive/googlecluster-ieee.pdf)

## Key ideas
* Commodity hardware
* Optimize for best aggregate throughput
* Replicate and parallelize to achieve that goal
* Price/performance beats peak performance in distributed systems

## Serving a query
* Multiple clusters of 1000+ nodes around the world
* DNS load balancing for user geoproximity
* processing of the query is entirely local to a cluster
* GWS (Google Web Servers) receive the query and format output into HTML

## Query execution phase 1
* Consult inverted index that maps words with sets of documents (hit list)
* Determine set of relevant documents by combining all hit lists and giving scores to the documents
* Scale of multiple TBs of data for both hit lists and the index
* MapReduce used to parallelize search by dividing the inde into multiple shards
* If shard replica is down ignore it and try to bring it back
* Result: list of docids, sorted

## Query execution phase 2
* Using list of docids, compute URIs and titles
* Document servers, called docservers fetch docs directly from disk
* MapReduce this operation
* Docservers effectively have access to a low latency version of the entire WWW

## Replication for capacity and fault tolerance
* System is optimized so that read-only is the only needed operation basically
* Divide query stream into multiple streams
* Because individual shards need not to communicate with each other:
  * more shards means faster service, almost linearly

## Power consumption and PC detail sections skipped
