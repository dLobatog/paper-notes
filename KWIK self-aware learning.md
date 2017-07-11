#![Knows What It Knows: A Framework For Self-Aware Learning](http://icml2008.cs.helsinki.fi/papers/627.pdf)

## History
* Polynomial sample complexity guarantees on other algorithms
* Sample complexity measurement using PAC, MB...

## Definition
* Knows What It Knows: relies on known/unknown partitioning
* Sample-efficient exploration
* Many models are KWIK-learnable

## Algorithm Definition
* Set of inputs X, output set Y
* Hypotheses class H, set of functions H -> Y mimicking the enviornment
* Target function h* belonging to H - unknown to the learner
* H and epsilon, delta are known to the learner and the environment

## Execution
* Environment selects an x belonging to X and tells the learner
* Learner predicts y_hat belonging to Y or "IDK"
* If y_hat =! IDK, y - y_hat <= epsilon where y = h*(x)
* If y_hat = IDK, the learner makes an observation z belonging to Z on the output where z=y in the deterministic case
  * non-deterministic cases omitted

## Properties
* Like PAC, KWIK cannot make mistakes
* Like MB, inputs to KWIK are selected adversarially
* Bound on number of label requests it can make

## Applications
* Memorization
  * Keep a mapping h_hat initialized to h_hat(x) = IDK for all x
  * When the environment chooses x the algorithm reports h_hat(x)
  * If h_hat(x) = IDK, get h_hat(x) = y
  * The KWIK bound is |X|
* Enumeration
  * Every time x belonging to X is provided by the environment
  * Compute L_hat = { h(x) | h belonging to H_hat }
    * If L_hat = 1 - all hypotheses for h(x) agree on the output, so return it
    * If L_hat != 1 - two hypotheses disagree. Request y belonging to Y from output and return IDK
      * Compute an updated H_hat = { h | h belonging to H_hat union h(x) = y }
* More omitted
