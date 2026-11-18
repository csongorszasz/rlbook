# About

My solutions to the programming exercises from the book [Reinforcement Learning: An Introduction (Second Edition), 2018, by Richard S. Sutton and Andrew G. Barto](http://incompleteideas.net/book/the-book-2nd.html).

# LLM usage

The solutions are the result of my own activity. LLMs were used only for visualizations, logging, or other boilerplate tasks. 

# Contents

- [Chapter 2: Multi-armed Bandits](#chapter-2-multi-armed-bandits)
- [Chapter 4: Dynamic Programming](#chapter-4-dynamic-programming)
- [Chapter 5: Monte Carlo Methods](#chapter-5-monte-carlo-methods)
- [Chapter 6: Temporal-Difference Learning](#chapter-6-temporal-difference-learning)
- [Chapter 7: n-step Bootstrapping](#chapter-7-n-step-bootstrapping)

# Chapter 2: Multi-armed Bandits

## $\epsilon$-greedy action selection on multi-armed bandits

Solutions to _Exercise 2.5_

### Reproducing solution to 10-armed stationary bandit

![](ch02/img/bandit_stat_mean-reward.png)

![](ch02/img/bandit_stat_optimal-action-ratio.png)

### Solution to the nonstationary bandit

> constant step-size ($\alpha=0.1$), $Var(Noise)=0.1$

![](ch02/img/bandit_nonstat_mean-reward.png)

![](ch02/img/bandit_nonstat_optimal-action-ratio.png)

---

Solution to _Exercise 2.11_

### Parameter study

> constant step-size ($\alpha=0.1$), $Var(Noise)=0.01$

![](ch02/img/paramstudy.png)

# Chapter 4: Dynamic Programming

## Policy iteration

Partial solutions to _Exercise 4.7_ (Jack's Car Rental problem)

### Optimal value function

$v_{min}=402.10287936952636$, $v_{max}=607.2791629089214$, $v_{mean}=538.9271065616259$

### Optimal policies

> After 1 iteration

![](ch04/carrental/policy_0.png)

> After 2 iterations

![](ch04/carrental/policy_1.png)

## Value iteration

Example solutions to _Exercise 4.9_ (Gambler problem)

### $p_{head}=0.25$

> Optimal value function

![](ch04/gambler/p_head_0.25/value_00.png)

> Optimal policy family

![](ch04/gambler/p_head_0.25/policy_00.png)
![](ch04/gambler/p_head_0.25/policy_01.png)
![](ch04/gambler/p_head_0.25/policy_02.png)

### $p_{head}=0.51$

> Optimal value function

![](ch04/gambler/p_head_0.51/value_00.png)

> Optimal policy

![](ch04/gambler/p_head_0.51/policy_00.png)

- the minor advantage of 0.01 probability leads to an ultrasafe betting strategy that wins over the long run

# Chapter 5: Monte Carlo Methods

## Learning near-optimal policies via Off-policy Monte Carlo Control

Example solutions to _Exercise 5.12_ (Racetrack problem)

### Example track nr. 1

Optimal trajectories with starting position $(31,6)$

> Noise turned OFF

![](ch05/racetrack/track_example_1_episodes-1000/optimal_path_start-31_6_noise-off.png)

> Noise turned ON: probability 0.1 of intented action being replaced by neutral action 

![](ch05/racetrack/track_example_1_episodes-1000/optimal_path_start-31_6_noise-0.1.png)

- car goes off-track twice - once, close to the finish, then again, immediately after being reset to the start at $(31,8)$

### Example track nr. 2

Optimal trajectories with starting position $(29,2)$

> Noise turned OFF

![](ch05/racetrack/track_example_2-episodes-10000/optimal_path_start-29_2_noise-off.png)

> Noise turned ON: probability 0.1 of intented action being replaced by neutral action 

![](ch05/racetrack/track_example_2-episodes-10000/optimal_path_start-29_2_noise-0.1.png)

- car goes off-track once, near the finish

[trained with NOISE=0.1]

# Chapter 6: Temporal-Difference Learning

## On-policy TD control (Sarsa)

Solutions to _Exercise 6.9_ (Windy Gridworld problem)

### Four possible actions (vertical+horizontal) _[reproducing book results]_

![](ch06/4actions/episodes_timesteps.png)
![](ch06/4actions/optimal_policy_trajectory.png)

### Eight possible actions (vertical+horizontal+diagonal)

![](ch06/8actions/episodes_timesteps.png)
![](ch06/8actions/optimal_policy_trajectory.png)

### Nine possible actions (vertical+horizontal+diagonal+noop)

![](ch06/9actions/episodes_timesteps.png)
![](ch06/9actions/optimal_policy_trajectory.png)

- found better path than with 8 actions - the no-op action helps obtain more accurate estimates of action values

---

Solutions to _Exercise 6.10_ (Stochastic Wind)

![](ch06/8actions-stochastic/episodes_timesteps.png)
![](ch06/8actions-stochastic/optimal_policy_trajectory.png)

# Chapter 7: n-step Bootstrapping

> IN PROGRESS

## n-step TD for value estimation

### _Solution to Exercise 7.2_: Comparing n-step TD using two different ways of computing the error term

Error term computation methods:

1. Online n-step TD: the value function changes across timesteps in an episode, boostrapping on live value estimates.

2. Sum of TD errors: the value function is assumed to update only at the end of an episode, bootstrapping on frozen episode-start value estimates.

![](ch07/7_2/rms_over_episodes_20260816_204358.png)

In the figure above, each line represents the root mean squared error (RMSE) computed from the estimated value function and the actual value function, over 100 episodes, averaged over 100 runs.

The TD-sum method (method nr. 2, dashed lines) appears to fall behind with higher error across nearly all (though non-exhaustive) tested configurations. One of the configurations, for some reason, spikes around episodes 60-80 ($\alpha=0.15$, n=1, dashed purple line).

The winner appears to be method nr. 1, online n-step TD.

### _Solution to Exercise 7.10_: Comparing data efficiency of off-policy learning with and without control variates

The learning curve of the algorithm with control variates is expected to converge quicker due to more stable variance.

In the following figures, a line represents the total reward obtained within an episode, averaged over 100 runs. The shaded regions show standard deviation.

\<FIGURE PLACEHOLDER\>
![]()

- Observation: the method (1) not using control variates degrades in performance drastically when large step size ($\alpha$) and large n-step are used, while the method (2) using control variates robustly achieves a positive score (though, far from optimal).

    - method (2) shows no further learning after the first few episodes, and gets stuck at a suboptimum; 
    
    - method (1), on the other hand, is able to find the optimal policy, though only through a very limited set of parameters
