# [DLRM - Deep Learning Recommendation Model](https://arxiv.org/pdf/1906.00091.pdf)

## Key ideas
* Personalization systems are used for tasks like CTR prediction and rankings. Two perspectives contributed to the current design of models for personalization, recommendation
  *  Content-filtering, where users selected their preferred categories and were matched on their preferences
  *  Collaborative filtering, where recs are based on past behaviors.
    *   Neighborhood methods that group users/products ina latent space were also deployed
  * Predictive models

## Design and Architecture 
* Embeddings: they map each category to a dense representation in an abstract space. They can map categorical features to a dense representation.
* Matrix factorization: 
  * ith product: w_i, jth user: v_j, m and n denote total number of products and users, r_i_j is the rating of the ith product by the jth user.  
  * <img width="158" alt="Screenshot 2022-09-19 at 16 45 13" src="https://user-images.githubusercontent.com/598891/191058284-b37329be-4275-4b90-8a38-814086ce5877.png">
* Factorization machine: 
  * Prediction function phi -> T, from input datapoint x in R, to target label y in T. 
* Multilayer perceptron:
  * Series of fully connected layers and activation function. 
* Users and products are described by many continuous and categorical features. Categorical features are represented by embeddings. Continuous features are transformed by an MLP to yield a dense representation of the same length as the embedding layer. 
* Second-order interactions - dotproduct of all vectors. Concatenate with the original processed dense features and post-process with output MLP, fed into sigmoid function to give a probability.

## Parallelism
* Embeddings contribute the majority of the parameters, with several tables requiring an excess of multiple GBs of memory. 
* MLP parameters are smaller in memory and translate into sizeable amounts of compute. 
* Data-parallelism is preferred for MLPs
<img width="365" alt="Screenshot 2022-09-19 at 16 58 35" src="https://user-images.githubusercontent.com/598891/191061041-c365f444-e20f-472c-8215-9fb797ba8b06.png">
