# [How attentive are GATs?](https://arxiv.org/pdf/2105.14491.pdf)

## Key ideas
* Rank of attention in GATv1 is unconditional on query node
* Because of this, some graph problems cannot be expressed by GAT

## Introduction
* Proof GAT doesn't compute a dynamic type of attention. It's only static.
  - Static means for any query node, the attention is monotonic with respect to its neighbors.
<img width="552" alt="Screenshot 2022-06-05 at 13 18 22" src="https://user-images.githubusercontent.com/598891/172050033-e6a94112-ba54-4e14-b3d5-2a1f7433ec98.png">

## Preliminaries
* Generic GNN layer: <img width="400" alt="Screenshot 2022-06-05 at 13 18 45" src="https://user-images.githubusercontent.com/598891/172050046-1158d00a-8477-483d-8695-243843242784.png">
* GAT attention scoring: <img width="400" alt="Screenshot 2022-06-05 at 13 19 13" src="https://user-images.githubusercontent.com/598891/172050065-1a33035d-d6c8-46f5-981c-6e80bbe20a44.png">
* GAT attention function: <img width="400" alt="Screenshot 2022-06-05 at 13 19 21" src="https://user-images.githubusercontent.com/598891/172050067-809ff85a-d0f2-4f15-994c-0377134772a7.png">
* GAT layer: <img width="400" alt="Screenshot 2022-06-05 at 13 19 24" src="https://user-images.githubusercontent.com/598891/172050068-cbd4ff95-cb04-4d74-86f0-d166b8483049.png">

## Static vs dynamic & limited expressivity of GAT
* Static attention:
  * Family of functions computing scoring for a set of key vectors and query vectors. For every f there's always a highest scoring key.
  * This is limiting because regardless of the query, every function has a key that's always selected
  * BUT different keys have different relevance to different queries in real problems. How to express this?
* Dynamic attention: 
  * Every key has different scoring based on the query node. Notice dynamic attention families will have strict subsets of static attention. 

## Need for a new scoring function
<img width="388" alt="Screenshot 2022-06-05 at 13 24 53" src="https://user-images.githubusercontent.com/598891/172050321-a80d9045-b419-4f9f-a40b-f2fa302dce0c.png">

* There exists a j_max wo that a_2(Wh_j) is maximal for all j in V.
* In that case, due to the monotonicity of LeakyReLU, for every query node i, the node j_max is leading to the maximal value of the distribution.
* To avoid this, we can apply the "a" learned attention layer *after* applying the LeakyReLU non linearity.
<img width="548" alt="Screenshot 2022-06-05 at 13 26 54" src="https://user-images.githubusercontent.com/598891/172050325-8c27b8f0-2522-4c15-8773-c8fba6c51a71.png">

## Evaluation
* Certain problems, like the following, cannot be learned with static attention (proven in appendix).
<img width="548" alt="Screenshot 2022-06-05 at 13 29 21" src="https://user-images.githubusercontent.com/598891/172050423-83ddc25e-adc7-415f-a966-34e26eb2e9e0.png">
