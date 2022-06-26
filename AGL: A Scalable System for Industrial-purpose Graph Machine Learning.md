# [AGL: A Scalable System for Industrial-purpose Graph Machine Learning](http://www.vldb.org/pvldb/vol13/p3125-zhang.pdf)

## Key ideas
* GNNs are inherently difficult to train in parallel because of the edge dependencies.
* Alternatives are storing the whole graph in memory (very expensive) or use third party storage (very slow)
* Instead: generate K-hop neighborhood, information complete subgraph for each node
* Merge values of in-edge neighbors, propagate to out-edge via MapReduce
* 2-layer GNN training with 2B nodes 100B edges in 14h.

## Introduction
* Heterogeneous graph in Ant Financial based on message passing, merging and propagation
* Computing each node’s K-hop embeddings based on message passing:
  * merging neighbors from in-edges and propagating merged information to neighbors along out-edges.
* Each K-hop neighborhood is a subgraph

## System
<img width="511" alt="Screenshot 2022-06-26 at 00 44 54" src="https://user-images.githubusercontent.com/598891/175793766-e75f09f0-5fe9-4ac8-9b90-366cea575849.png">

* GraphFlat is an efficient and distributed generator, based on message passing for generating K-hop neighborhoods. 
  * These neighborhoods are flattened into protobuf strings.
*  GraphTrainer leverages many techniques, such as pipeline, pruning, and edge-partition, to eliminate the overhead on I/O and optimize the floating point calculations 
*  GraphInfer, a distributed inference module that splits K layer GNN models into K slices, and applies the message passing K times based on MapReduce.   
  *  GraphInfer maximally utilizes the embedding of each node because all the intermediate embedding at the k-th layer will be propagated to next round of message passing. This significantly boosts the inference tasks
* We can divide an industrial-scale graph into massive of tiny K-hop neighborhoods w.r.t. their target nodes in advance, and load one or a batch of them rather than the entire graph into memory in the training phase

<img width="1032" alt="Screenshot 2022-06-26 at 15 54 41" src="https://user-images.githubusercontent.com/598891/175820267-115cd1b0-7178-46b2-a53a-3ab956a21d2e.png">
<img width="553" alt="Screenshot 2022-06-26 at 15 56 08" src="https://user-images.githubusercontent.com/598891/175820311-392aea5d-84db-4be7-9b61-b6d4f2e73f06.png">
<img width="1049" alt="Screenshot 2022-06-26 at 15 57 18" src="https://user-images.githubusercontent.com/598891/175820350-1f2864a1-c7e6-4ac8-a633-00f588a214d6.png">
