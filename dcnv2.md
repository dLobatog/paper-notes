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

