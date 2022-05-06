# [Do Transformers Really Perform Bad for Graph Representation?](https://arxiv.org/pdf/2106.05234.pdf)

## Key ideas
* Transformer architecture is becoming a dominant choice in NLP and CV, why not in Graphs?
* Solution: Graphormer which uses special positional encoding for graphs.
* Problem not mentioned in the paper: graph "positional encoding" often requires shortest path computation between two nodes.

## Introduction
* Transformer is the most powerful NN in modeling sequential data such as speech and natural language processing 
* It's an open question whether Transformers are suitable to model graphs - Graphormer seems to be an affirmative answer
* Most leaderboards and benchmarks in chemistry seem to do well
* Centrality encoding captures node importance in the graph
* Spatial encoding captures the structural relation between nodes - for each node pair, we assign a learnable embedding based on the spatial relation.

## Preliminary 
* GNN aim to learn representation of nodes and graphs.
* Modern GNNs mfollow a learning schema that updates the representation of an ode by aggregating representations of its neighbors.
* Aggregate-combine step: 
