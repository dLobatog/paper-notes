# [How attentive are GATs](https://arxiv.org/pdf/2105.14491.pdf)

## Key ideas
* Rank of attention in GATv1 is unconditional on query node
* Because of this, some graph problems cannot be expressed by GAT

## Introduction
* Proof GAT doesn't compute a dynamic type of attention. It's only static.
  - Static means for any query node, the attention is monotonic with respect to its neighbors.
 
## Preliminaries
