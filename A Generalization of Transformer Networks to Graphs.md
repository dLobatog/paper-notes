# [A Generalization of Transformer Networks to Graphs](https://arxiv.org/pdf/2012.09699v2.pdf)

## Key ideas
* Original transformer was conceived for NLP which oeprates on graphs representing all connections between words in a sequence.
* Original transformer does not leverage graph inductive bias.
* New graph transformer introduces four properties vs original one. 
  * Attention mechanism is a function of neighborhood connectivity for each node
  * Positional encoding is represented by Laplacian eigenvectors, which generalize the sinusoidial positional encodings in NLP
  * Layer normalization is replaced by batchnorm layer.
  * Architecture is extended to edge feature representation (e.g: applied in chemistry for bond type) or link prediction.



References:
[Relational inductive biases, deep learning, and graph networks](https://arxiv.org/pdf/1806.01261.pdf)
