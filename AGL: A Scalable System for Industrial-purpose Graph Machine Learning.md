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

## Model
