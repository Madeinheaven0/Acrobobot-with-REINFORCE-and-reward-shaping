# AGENT TRAINING FOR THE ACROBOT-V1 ENVIRONMENT - EXPERIMENTATION OF THE REWARD SHAPING

## Summary
1. INTRODUCTION
2. Description of the Acrobot-v1 environment
3. Description of reward shaping
4. RESULTS 
5. CONCLUSION


## **INTRODUCTION**

This project is made with the objectif to show you the REINFORCE algorithm and the advantages of use the reward shaping in some cases like that you have now.

## I- **Description of the Acrobot-v1 environment**

### 1- Concept

Acrobot is a classic problem of control in robotic and reinforce learning. The name is a contraction `Acrobatic Robot`.
Let's think a marionette with 2 links suspended in a fix point. The first link is fixed to that point and the second is fixed to the extremity of the first.
The goal is simple to say but hard to realize: acrobate the 2 links with the objective that que extremity of the second link reach a specific height.

### 2- Spaces

- States space:
  The observation is a vector of 6 elements ( np.array), who contains the following walues:
1. $\cos{\theta_1}$
2. $sin{\theta_1}$
3. $\cos{\theta_2}$
4. $\sin(\theta_2)$
5. angular speed of theta 1
6. angualr speed of theta2

- Where:
  - theta1 is the angle between the first link and the vertical
  - theta2 is the angle between the second link and the first link

- Actions spaces:
  Is a discrete space with 3 possible actions:
  - 0: Applicate a negative couple
  - 1: Don't applicate a couple
  - 2: Applicate a positive couple

### 3- Dynamism and reward

- Dynamism: The physic of the game is simulated by differential equations of the movement of a double pendulum with the gravitationnal action. The environment use the euler's integrator to evoluate the system time step by time step.
- Rewards:
  - Minus 1 is given for each time step until the ga=oal is reached
  - O if te goal is reached

It means that's the agent is strongly incited to resolve the problem quiqly, cuz his objective is to maximize the recompense (minimize the sum of -1).

### 4- Conditions of ending

The episode is terminated only if the goal is reached. The goal is defined as:

$$\cos{\theta_1} - \cos{\theta_2 + \theta_1} > 1.0$$



## II- **Reward Shaping**

The reward shaping is a reinforcement learning technique who consist to modify the reward of the environment to guide the agent. His main purpose is to make easy the learning by providing intermediate rewards.

### 1- The problem

In the most existing environments, the reward is sparse(rare). It's mean that the agent received positive reward only if it has reached the goal after a long actions sequence.
This absence of feedback make the learning very hard because the agent don't know which actions helps him for the task.
The reward shaping add an intrinsic reward to the extrinsic reward reward (for the environment). That new reward is a potential reward of the environment state who helps the agent to be near the goal.
Mathematically, the total reward $R_{total}$ is the sum of the original reward and the shaping reward $F$.
$$R_{total}(s, a, s') = R_{extrinsic}(s, a, s') + F(s, s')$$

The reward shaping $F$ is often defined like the potential difference between the new state $s'$ and the precedent $s$.
$$F(s, s') = \gamma\Phi(s') - \Phi(s)$$

where $\Phi$ is a potential function who measures the proximity of the state $s$at the objective, and $\gamma$ the discount factor.

#### The goal of reward-shaping
1. Speedup the learning
2. Make easy the observation
3. Stabalization become better

#### How do we designed the reward-shaping

Our purpose is to reach a specific height. So Our reward shaping is based on the height of the second link at each time step. 
We use the cosine and sinus of the $\theta_1$ and $\theta_2$ to compute the height.
The formula is:

$$y_2 = link-lenght_2 *\cos_{total} $$ 
where:
$$\cos_{total} = \cos{\theta_1 + \theta_2} = \cos{\theta_1}*\cos{\theta_2} -  \sin{\theta_1}*\sin{\theta_2}$$

$$y1 = -link-lenght_1 * \cos{\theta_1}$$

The final reward is:
$$R_{total} = R_{initial} + R_{shaping}$$
where:
$$R_{shaping} = \gamma * y_2$$

## III- **Results**

I you don't use the reward shaping the agent will turn around and randomly acts without knows which action is good and which are bad. It's necessary to run incredible number of episodes to have the luck to acomplish the purpose and even there, the agent doesn't know the sequence of agents who reached them to this result.
By using the reward shaping, we can accomplish the goal in 25 steps.This result can be improve by tuning parameters like the learning rate, discount factor of the return, the number of layer of the actor-critic network and the nber of neurons by layer etc.