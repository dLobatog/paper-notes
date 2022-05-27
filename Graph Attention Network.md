# [Graph Attention Networks](https://arxiv.org/pdf/1710.10903.pdf)

## Key ideas
* Apply different weights to different nodes in the neighborhood without prior knowledge of the graph's structure
* Spectral vs non-spectral approaches to graph convolutions
  - Spectral filters learn on Laplacian eigenbasis, hence they are dependent on graph structure
  - Non-spectral filters use convolution difrectly on the graph operating as close neighbors
    - Sampling a fixed-size neighborhood (weighted for attention, self link for self-attention) and aggregatin3 Attention g 

## Attention Layer
* input: node features h={h1,h2,h3... (vectors)}
* output: ditto
* At least one linear transformation is required to have higher level features. A weight matrix 'W' can be used for this.
* <img width="150" alt="Screenshot 2022-05-27 at 16 43 31" src="https://user-images.githubusercontent.com/598891/170800685-d2af3860-abda-4939-a6ae-ae66c0fdb6ee.png">
* <img width="317" alt="Screenshot 2022-05-27 at 16 43 43" src="https://user-images.githubusercontent.com/598891/170800707-f7d91d20-b8d2-4b81-854f-93eae973c798.png">
* Where eij is the importance of node j's features to node i
  - we only compute it for nodes j in i's neighborhood.
* Also || is concatenation
* To stabilize the process of self-attention we have found extending one mechanism to employ multi-head attention
<img width="563" alt="Screenshot 2022-05-27 at 16 45 49" src="https://user-images.githubusercontent.com/598891/170800782-632b2e81-c78d-4dad-92fe-4d9a21aaf797.png">
<img width="184" alt="Screenshot 2022-05-27 at 16 46 10" src="https://user-images.githubusercontent.com/598891/170800794-903a312b-1d6a-4a2a-aa6b-294e00567f60.png">

* Highly efficient as computing attention can be parallelized across multiple edges in O(|V|FF' + |E|F') where F is # of node features. 
