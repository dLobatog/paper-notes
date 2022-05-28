# [How Powerful Are Graph Neural Networks?](https://arxiv.org/pdf/1810.00826.pdf)

## Key points
* Framework for analyzing expresiveness and discriminative power of GNNs
* Weisfeiler-Lehman Isomorphism test is NP-complete. We need to iteratively update node's feature vector by aggregating the neighborhood's feature vectors. 
* Set of node's neighborhood feature vectors: multiset with possibly repeating elements
* GNNs proven to be AT MOST as powerful as WL-test in distinguishing graph structures
* Conditions for the above established in the paper\
* GIN: Graph Isomorphism Network shows representative power is equal to WL-test

## Building powerful GNNs

<img width="542" alt="Screenshot 2022-05-27 at 17 39 51" src="https://user-images.githubusercontent.com/598891/170802904-0c642cc4-9a0f-479f-969b-420d4e7ba970.png">

* To study the representational power of a GNN, we analyze when a GNN maps two nodes to the same location in the embedding space.
   * Intuitively, a maximally powerful GNN maps two nodes to the same location
   * only if they have identical subtree structures with identical features on the corresponding nodes.
* A maximally powerful GNN would never map two different neighborhoods to the same representation!

<img width="542" alt="Screenshot 2022-05-27 at 17 57 57" src="https://user-images.githubusercontent.com/598891/170803468-84aadcfa-cfc1-4097-8120-e9649677359f.png">

## GIN (Graph Isomorphism Network)
* UPDATE step:
* <img width="472" alt="Screenshot 2022-05-27 at 18 04 45" src="https://user-images.githubusercontent.com/598891/170803665-08e2447a-74da-4641-b109-ecc88ade831d.png">

* Analysis of different aggregators:
<img width="485" alt="Screenshot 2022-05-27 at 18 06 11" src="https://user-images.githubusercontent.com/598891/170803706-1b877008-707e-4213-8ac2-d2bb49bafcd8.png">

* Notice sum captures the full multiset, mean captures the proportion/distribution of elements of a given type, and the max aggregator ignores multiplicities
* <img width="391" alt="Screenshot 2022-05-27 at 18 07 33" src="https://user-images.githubusercontent.com/598891/170803767-41481e1b-b4f8-41c0-b8b1-db692a90efed.png">

* Hence the GIN is maximally powerful when it can distinguish multisets, using "SUM" as the READOUT step in the previous equation.
