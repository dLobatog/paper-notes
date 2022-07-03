# [DistDGL: Distributed Graph Neural Network Training for billion-scale graphs](https://arxiv.org/pdf/2010.05337.pdf)

## Key ideas
* Based on the DGL (Deep Graph Libary)
* Synchronous-training approach, ego-networks forming mini-batches
* Min-cut partitioning algorithm.

## Introduction
* Challenging to train GNNs because of the inherent dependencies between nodes (vertices) - next node can be on a completely different machine
* Each mini-batch must incorporate depending samples (1-hop, 2-hop, etc..). Sampling algo is important.
* As opposed to regular distributed NN training where the majority of network traffic comes from exchanging gradients, distributed GNN traffic comes from reading hundreds of neighbor vertices.

## Background
* Mini-batch training: 
  * Sample a set of N vertices, uniformly at random frmo the training set
  * Pick at most K neighbor vertices (fan-out) for each target
  * Compute the target vertex representations by gathering messages
<img width="357" alt="Screenshot 2022-07-03 at 13 47 51" src="https://user-images.githubusercontent.com/598891/177040417-26506169-a8a6-4682-8675-945ff299f921.png">

## System design DistDGL
* Samplers: sample the mini-batch graph structures from the input graph
* KVStore: distributed store for all vertex and edge data (so mini-batches can find each other when necessary)
  * Special kv-store instead of Redis or similar because we need better co-location of node/edge fatures in KVStore and graph partitions
  * Efficient updates for sparse embeddings
  * Flexible partition politices to map data to different machines 
  * Supports sparse embedding for training transductive models with learnable vertex embeddings
* Trainers: 
  * 1st: fetch the mini-batch graphs from the samplers and corresponding vertex/edge features from KVStore
  * 2nd: run forward/backward in parallel
  * 3rd: dense model update component synchronized

<img width="326" alt="Screenshot 2022-07-03 at 13 51 24" src="https://user-images.githubusercontent.com/598891/177040532-34ded721-7b64-4075-9d24-7bc78a2a925e.png">

* Graph partitioning: Uses [METIS](https://en.wikipedia.org/wiki/METIS)
  * Densely connected vertices are assigned to the same partition to reduce the number of edge cuts
  * Minimizing edge-cut improves GNN performance as we need to check the KVStore less and less.

<img width="326" alt="Screenshot 2022-07-03 at 13 53 00" src="https://user-images.githubusercontent.com/598891/177040588-0a754ee9-bad2-46c5-ad86-950369a5b158.png">

