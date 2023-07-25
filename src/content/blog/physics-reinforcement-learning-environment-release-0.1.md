---
title: 'Physics based reinforcement learning environment (Release 0.1)'
description: 'Physics based reinforcement learning environment (Release 0.1)'
pubDate: 'Jul 25 2023'
---

I've been working on a [physics based reinforcement learning environment with a world editor](https://github.com/ShouvikGhosh2048/physics_reinforcement_learning_environment) and published [the first release](https://github.com/ShouvikGhosh2048/physics_reinforcement_learning_environment/releases/tag/0.1) a few days ago.

# The world editor
![World editor](/website/physics-reinforcement-learning-environment-1.PNG)

The world consists of the player, blocks the player can interact with and goals to reach.

Current editor features:
- Opening and saving worlds
- Scale and rotation gizmos
- Zoom in/out while editing the world, centered at the mouse cursor
- Playing the world

# Training
Currently only Genetic Algorithms have been implemented.
For genetic algorithms, you can choose:
- Number of agents in each generation
- Number of times to repeat a move: The physics environment currently plays each move for 1/60th of a second. Repeating a move allows the move to be played over a longer duration of time, thus making larger movements likelier (compared to multiple left-right moves, each played over a small duration of time and thus the player only moving a small distance).

# Libraries used
This project is built in Rust. 

It uses: 
- Bevy for the game engine
- egui (used by bevy_egui) for the GUI
- Rapier for physics
- serde for world serialization/deserialization
- rfd for file dialogs
- crossbeam for channels (used in multithreading - the agent is trained on a seperate thread)
- rand for randomness

The project also uses [Bevy Github CI Template](https://github.com/bevyengine/bevy_github_ci_template) for Github workflows.