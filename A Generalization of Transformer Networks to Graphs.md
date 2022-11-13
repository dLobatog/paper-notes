# [A Generalization of Transformer Networks to Graphs](https://arxiv.org/pdf/2012.09699v2.pdf)

## Key ideas
* Original transformer was conceived for NLP which oeprates on graphs representing all connections between words in a sequence.
* Original transformer does not leverage graph inductive bias.
* New graph transformer introduces four properties vs original one. 
  * Attention mechanism is a function of neighborhood connectivity for each node
  * Positional encoding is represented by Laplacian eigenvectors, which generalize the sinusoidial positional encodings in NLP
  * Layer normalization is replaced by batchnorm layer.
  * Architecture is extended to edge feature representation (e.g: applied in chemistry for bond type) or link prediction.


## Architecture
### On Graph Sparsity
* On NLP, a sentence is a treated as a FC graph. This choice is justified because it's difficult to find sparse interactions among words in a sentence. The dependency of a word on another word varies with context, applications.. 
* This graph for NLP transformers has less than 10s or 100s of nodes (sentences can only be so long). This makes it computationally feasible. Actual graph datasets can have node sizes of billions.
* On these accounts, it's more practical to have a Graph Transformer where a node attends to local node neighbors. 

### On Positional Encodings
* NLP positional encoders need to ensure unique representation for each word and preserve distance information. 
* The design of unique node positions is challenging on graphs, as there are symmetries and isomorphisms that make graphs invariant to the node position.
  * This is a critical reason why GAT, where attention is a function of local neighborhood connectivity, instead of full-graph connectivity, does not achieve competitive performance on graph datasets. 
  * Other literature cover Laplacian Positional Encoders as a generalization of the sinusoidial ones in original Transformers. 
  * They encode distance-aware information: nearby nodes have similar positional features, farther nodes have dissimilar ones.
* Eigenvectors are defined as the factorization of the graph Laplacian matrix:
  <img width="341" alt="Screenshot 2022-11-13 at 13 31 48" src="https://user-images.githubusercontent.com/598891/201524349-db2dcef2-4022-4224-b295-b8ff89b5eab5.png">

### Graph Transformer Architecture
* Very similar to the original Attention is all you need architecture.
<img width="720" alt="Screenshot 2022-11-13 at 13 17 56" src="https://user-images.githubusercontent.com/598891/201523676-1a1195c7-290d-4cb7-95d7-7cce3626f15d.png">

* Node updates
<img width="341" alt="Screenshot 2022-11-13 at 13 37 13" src="https://user-images.githubusercontent.com/598891/201524619-135340b4-50a8-4517-b039-2c28116fdf6e.png">

* Attention outputs passed to Feed-Forward-Network preceded and succeeded by residual connections:
<img width="341" alt="Screenshot 2022-11-13 at 13 38 19" src="https://user-images.githubusercontent.com/598891/201524676-ab87a4c9-e30a-4e3c-b48e-7ed16902b4a5.png">

* Check the original paper for the edge-features version. 

## Discussion
* Generalization of transformer network on graphs works best when Laplacian PE are used, and BatchNorm is used instead of LayerNorm.
* Proposed architecture outperforms baseline isotropic and anisotropic GNNS (GCN and GAT).
  *  Fresh and improved attention-based GNN baseline surpassing GAT.
* Sparse graph connectivity is a critical inductive bias for datasets with arbitrary graph structures. Demostrated comparing sparse vs full graph experiments. 

References:
[Relational inductive biases, deep learning, and graph networks](https://arxiv.org/pdf/1806.01261.pdf)
