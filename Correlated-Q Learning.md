#[Correlated Q-learning](https://pdfs.semanticscholar.org/7480/4472b338283a5b92659db76b55f9eaef8b74.pdf)

## Key ideas
* Generalize Nash Q and Friend-Foe Q (FFQ)
* Empirical convergence to equilibrium policies of general-sum repeated games

## Introduction
* Goal: learn equilibrium policies in general-sum markov games
* Nash-Q: only works on some games, depending on conditions
* FF-Q: same
* Correlated-Q:
  * In general-sum games, correlated equilibria contains Nash
  * In constant-sum games, correlated equilibria contains minimax
* Nash equilibrium
  * Vector of independent probability distributions over actions
  * All agents optimize with respect to one another probabilities
  * No agent would prefer a different action at convergence
* Correlated equilibrium (more general)
  * Dependencies among agents probability distributions
  * Can be computed using linear programming
* Difficulties for learning equilibrium policies
  * Multiple equilibria - multiple payoff values
  * Equilibrium selection via four selection functions (utilitarian, egalitarian, republican, libertarian)

## Markov games
* Generaliation of repeated games & MDP (stochastic)
* Stochastic games are a tuple:
  - I, set of players
  - S, state
  - Ai(s) where s belongs to S, i to I, ith player available actions at state s
  - P, probability transition function
  - R(i) reward for ith player

## Q in Markov games
* State-action vectors instead of state-action pairs
* Qi(s, a) = (1-gamma) * Ri(s, a) + gamma * sum(P(s' given s,a) * Vi(s'))
* all players maximizing their own rewards is not an adequate strategy

## Friend Q
* All the players reward functions are the same, 2-player
* Vi(s) = max Qi(s, a)

## Nash Q
* For n-player, general sum games
* Vi(s) = Nash for ith player (Qi(s)...Qn(s))
* Nash for ith player denotes ith player according to Nash equilibrium determined by reward matrices

## CE-Q
* Correlated equilibrium allows for dependencies in the agents' randomizations:
* Probability distribution over the joint space of actions, agents optimize with respect to one another but taking themselves into account ultimately
* Utilitarian
  * maximize the sum of all players' rewards
  * argmax sum of players rewards
* Egalitarian
  * maximize the minimum of all players' rewards
  * argmax min
* Republican
  * maximize the maximum of all players' rewards
  * argmax max
* Libertarian
  * maximize the maximum of each individual ith player rewards
  * argmax rewards where result is a Correlated Equlibrium
