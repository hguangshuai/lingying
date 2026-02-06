# Reinforcement Learning Basics

## English Description / Topic
Introduction to the core concepts of **Reinforcement Learning (RL)**, focusing on the relationship between Agent, Environment, State, Reward, and Penalty.

## Fundamental Knowledge

### 1. Key Components of RL
Reinforcement Learning is about learning what to do—how to map situations to actions—so as to maximize a numerical reward signal.
- **Agent**: The learner or decision-maker (e.g., a robot, a game player).
- **Environment**: Everything the agent interacts with.
- **State ($S$)**: A representation of the current situation of the agent within the environment (e.g., coordinates on a map, board configuration in chess).
- **Action ($A$)**: All possible moves the agent can make.
- **Reward ($R$)**: A numerical value returned by the environment after an action. It tells the agent how "good" or "bad" the action was in the context of the goal.
- **Penalty**: Essentially a negative reward. It is used to discourage the agent from taking certain actions (e.g., hitting a wall, losing a point).
- **Policy ($\pi$)**: The agent's strategy or rulebook for deciding which action to take based on the current state.

### 2. The RL Loop
1. The Agent perceives the current **State** $S_t$.
2. The Agent performs an **Action** $A_t$ based on its **Policy**.
3. The Environment transitions to a new state $S_{t+1}$ and provides a **Reward** $R_{t+1}$ (or **Penalty**).
4. The Agent updates its knowledge to maximize the **cumulative future reward**.

### 3. Exploitation vs. Exploration
- **Exploration**: Trying new actions to discover more about the environment.
- **Exploitation**: Using the best-known actions to maximize the reward.
- A successful RL agent must balance these two.

## Spoken / Interview Answer
"Reinforcement Learning is a type of machine learning where an **Agent** learns to make decisions by interacting with an **Environment**. 
The process is guided by a feedback loop: the agent observes the current **State**, takes an **Action**, and receives a **Reward** or a **Penalty**. The goal of the agent is to learn a **Policy** that maximizes the total reward over time. 
For example, in a self-driving car, a 'Reward' could be reaching the destination safely, while a 'Penalty' would be triggered by traffic violations or collisions. Unlike supervised learning, we don't give the agent the 'correct' answer; it has to figure out the best strategy through trial and error."


