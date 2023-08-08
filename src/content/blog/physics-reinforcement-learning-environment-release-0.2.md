---
title: 'Physics based reinforcement learning environment (Release 0.2)'
description: 'Physics based reinforcement learning environment (Release 0.2)'
pubDate: 'Aug 8 2023'
---

My [physics based reinforcement learning environment with a world editor](https://github.com/ShouvikGhosh2048/physics_reinforcement_learning_environment) had it's [0.2 release](https://github.com/ShouvikGhosh2048/physics_reinforcement_learning_environment/releases/tag/0.2) yesterday.

# World editor improvements
- Option to create a new world
- Delete button in the selected object menu

# Training
For genetic algorithms the following options have been added:
- mutation_rate: Each move of an agent consists of three booleans - whether to press the up key, left key and right key. When we mutate the moves of an agent, we go through each boolean of each move and with a probability mutation_rate decide to randomly reassign this boolean.
- keep_best: Whether to keep the best player from the last generation.

DQN agents have also been added. They use [dfdx](https://github.com/coreylowman/dfdx) and their implementation has been taken from [here](https://github.com/coreylowman/dfdx/blob/main/examples/rl-dqn.rs) and [here](https://pytorch.org/tutorials/intermediate/reinforcement_q_learning.html). They're still a work in progress. Currently they can only be used with worlds with no dynamic blocks and their input is the current player position and velocity.

# Contributing
I've added a few [issues](https://github.com/ShouvikGhosh2048/physics_reinforcement_learning_environment/issues) to the repository for anyone intreseted in contributing.