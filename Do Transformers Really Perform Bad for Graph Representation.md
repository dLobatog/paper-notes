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

![Screenshot 2022-05-06 at 19 16 09](https://user-images.githubusercontent.com/598891/167196013-a49a9871-a326-4ad1-967f-23d76fed3c21.png)

## Preliminary 
* GNN aim to learn representation of nodes and graphs.
* Modern GNNs mfollow a learning schema that updates the representation of an ode by aggregating representations of its neighbors.
* Aggregate-combine step: ![Screenshot 2022-05-06 at 19 22 15](https://user-images.githubusercontent.com/598891/167196155-aed933ca-bd96-459f-b45b-0f0cc22a732b.png)
* The task: graph classification

## Centrality Encoding
* Learnable embedding applied to each node based on the indegree/outdegree of them
* ![Screenshot 2022-05-06 at 19 23 39](https://user-images.githubusercontent.com/598891/167196315-577ebfab-86dc-48ec-b503-f90075476704.png)

## Spatial Encoding
* Biased term is specific to the shortest path distance between v_i and v_j.
*![Screenshot 2022-05-06 at 19 24 37](https://user-images.githubusercontent.com/598891/167196447-24310972-80fc-4e4b-abea-9fade889157f.png)

## Special node
* A Node \[VNode\] is added to the graph which is then connected to every individual node (distance of 1). The point is that hte representation of the entire h_g graph is the node feature of \[VNode\]
