
#MDP

MDP is a tuple <S, A, P, R>, where
S - set of states of the world
A - set of actions
P: SxA -> S, state transition function
R: SxA -> R, reward function

Markov property

Gole: Solve MDP by finding an optimal policy

1. What is objective?
2. How to evaluate objective?
3. How to improve objective?
4. Combine evaluation policy and improvement

Cumulative reward is called `return` `G`.:
$$G = R\_t + R\_{t+1} + ... + R\_T$$

$R\_t$ - награда в момент времени `t`. 

Reward disconting

Get rid of infinite sum by disconting $0 <= \gamma < 1$. $gamma$ - disocounting factor.

$$G = R\_t + \gamma R\_{t+1} +  \gamma^2 R\_{t+1}... = \sum \gamma^k R\_{t+k+1}$

Reward today is better than reward tomorrow. \gamma задает скорость обесценивания будущих наград. Any discounting changes optimization task and solution.

Mathematical conveniance

$$G\_t = R\_t + \gamma (R\_{t+1} + \gamma R\_{t+2} + ...) = $$
$$G\_t = R\_t + \gamma G\_{t+1}$$

Takeway: Reward only for `WHAT` but never for `HOW`

State/Action value functions

State-Value function v(s) is expected return conditional on state
Action-Value function q(s, a) is expected return conditiona on state and action

Bellman `expectation` equations for $v(s)$, $q(s, a)$.
Bellman `optimality` equations for $v(s)$,  $q(s, a)$.

Optimal policy is one with the largest $v(s)$.

General policy iteration:
1. Policy Evaluation - predict value function for a partiular policy, Bellman `expectation` equation $v\_{\pi}(s)$.
2. Policy Improvement - $\pi'(s) <- argmax\_a q\_{\pi}(s, a)$


Generalized policy iteration:
- Policy iteration
  1. Evaluate policy until converge (with some tolerance)
  2. Improve policy
- Value iteration
  1. Evaluate policy with single iteration
  2. Improve policy
