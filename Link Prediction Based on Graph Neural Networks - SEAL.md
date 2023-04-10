# [Link Prediction based on Social Networks - SEAL](https://arxiv.org/pdf/1802.09691.pdf)

## Key ideas
* Prior art based on common neighbors and Katz index for likelihood of links.
* Learning the heuristic from the network instead of using predfined ones should be better
* Developed a novel gamma-decaying heuristic theory.
* Local subgraphs are proven to have rich information for link existence.

## Introduction
* Heuristics like common neighbors and preferential attachment are first-order heuristics: they involve 1-hop.
* Heuristics like Adamic-Adar (AA), Resource Allocation (RA) are 2nd order heuristics.
* h-order heuristics go to the h-hop neighborhood.
* <img width="905" alt="Screenshot 2023-04-09 at 23 30 20" src="https://user-images.githubusercontent.com/598891/230799505-47159657-8839-44f0-8f03-92cbe5a686a1.png">
* These heuristics belong to a more generic class: graph structural features.
  * Such are any features which can be computed directly from the graph.
* Weisfeiler-Lehman Neural Machine (WLNM) studied this problem and provided features specific for link prediction.
* Higher order heuristics like PageRank and Katz have better performance than first and second order ones.
* We don't need a huge h number for h-heuristics.
* SEAL is an improvement on WLNM

## Preliminaries
* Latent features: factorize a matrix representation to learn a low-dimensional representation for each node. Node2vec, deepwalk, matrix factorization..
* Explicit features: usually node attributes.
* Combining both usually brings out the best performance
* Supervised heuristic learning: WLNM trained FCNN on the subgraphs adjacency matrices.

<img width="905" alt="Screenshot 2023-04-09 at 23 48 49" src="https://user-images.githubusercontent.com/598891/230800020-de943c7e-592a-4634-ad71-840be2c87745.png">
