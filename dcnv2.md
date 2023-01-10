# [DCN V2: Improved Deep & Cross Network and Practical Lessons for Web-scale Learning to Rank Systems](https://arxiv.org/pdf/2008.13535.pdf)

## Key ideas
* Learning effective feature crosses is key behind building recommender systems. 
* Moving form Factorization Machine (FM) methods to NNs, didn't work as they don't approximately model 2nd or 3rd-order feature crosses.
* Making NNs wider and deeper isn't a solution, as this makes them much slower to serve. Can't handle high QPS.
* DCN aims to leverage **implicit** high-order crosses from NNs, with **explicit** crosses modeled by formulas with controllable interaction order.
* DCN cross network is O(input size), limiting flexibility. 
* DCN-V2 first learns explicit feature interactions through cross layers, and then combines with a deep network to learn complementary implicit interactions.

## Related work
* Parallel Structure
  - Jointly train two parallel networks. Inspired from wide and deep model.
  - Wide component takes inputs as crosses of raw features. 
  - Deep component is a NN
  - Examples are DeepFM or DCN
* Stacked Structure
  - Introduce interaction layer which creates explicit feature crosses between embeddings and DNN

## Proposed Architecture: DCN-V2
* Significantly improve expresiveness of DCN in modeling complex explicit cross terms, with easy deployment.
* Observing the low-rank nature of the cross layers, we propose to leverage a mixture of low-rank cross layers 
<img width="660" alt="Screenshot 2023-01-10 at 12 15 43" src="https://user-images.githubusercontent.com/598891/211549266-35ccf743-db77-4539-b737-323e48e18518.png">

* Embedding Layer
  - takes combination of sparse (categorical) and dense features and outputs x_0.
  - i-th categorical feature is projected from high-dimensional sparse sapce, to low-dimensional dense space through learned projection matrix. 
  - output is the concatenation of all embedded vectores and normalized dense features
 
 
* Cross Network
<img width="684" alt="Screenshot 2023-01-10 at 12 16 08" src="https://user-images.githubusercontent.com/598891/211549347-4ca9ab67-8120-4d85-81a3-7126d71128d5.png">

* Deep Network is just an NN activated by ReLU. Other activation functions are suitable.

## Combination of deep and cross networkks.
- Stacked: x0 input is fed to cross network, then is fed to deep network. f_deep * f_cross is the output
- Parallel: x0 is fed in parallel to both cross and de
ep networks. f_deep + f_cross is the output 
<img width="367" alt="Screenshot 2023-01-10 at 12 21 51" src="https://user-images.githubusercontent.com/598891/211550620-800afe7e-92c2-4dcb-9e3e-7b448858c96d.png">

## Cost-Effective Mixture of Low-Rank DCN
* [Low-rank techniques](https://en.wikipedia.org/wiki/Low-rank_approximation) are used to reduce computational cost.
* Approximates a dense matrix M by two tall and skinny matrices U, V. 
* Most effective when matrix shows a large gap in singular values of fast spectrum decay. 
<img width="580" alt="Screenshot 2023-01-10 at 12 30 11" src="https://user-images.githubusercontent.com/598891/211552187-ed8df31c-55c5-4404-98f6-ff8e568e3694.png">
