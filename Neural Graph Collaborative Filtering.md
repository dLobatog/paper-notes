# [Neural Graph Collaborative Filtering](https://arxiv.org/pdf/1905.08108.pdf)

## Key ideas
* The collaborative signal is typically not encoded in regular embeddings for users or items.
* Proposal to capture bipartite graph into the embedding process.

## Introduction
* 2 key components in learnable CF models.
*  - Embeddings which transform user/items into vectorised representations
*  - Interaction modeling which reconstructs historical interactions based on the embeddings.
* Matrix Factorisation for example directly embeds user/item ID as a vector and models interactions via inner product.
* Neural CF replace this inner product with non-linear NN.
* Translation CF replaces inner product with distance metrics like Euclidean
* <img width="367" alt="Screenshot 2022-12-23 at 21 18 07" src="https://user-images.githubusercontent.com/598891/209406341-07838a9a-8d8c-4478-b03e-cff26689d3b8.png">

* Instead of expanding the interaction graph as a tree, which is complex and expensive:
*  - Devise an embedding propagation layer which aggregates embeddings of interacted items or users.
*  - Stack multiple embedding propagation layers

## Methodology
* Embedding layer: builds a parameter matrix as an embedding lookup table. This is the intiial state of user and item embeddings to be optimised.
* <img width="367" alt="Screenshot 2022-12-23 at 21 26 17" src="https://user-images.githubusercontent.com/598891/209406826-fc1a973b-b7c2-47e9-acbc-274412a5482a.png">
* Embedding Propagation Layesr
