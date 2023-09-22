---
title: 'Brute Force Search with bins'
description: 'Brute Force Search with bins'
pubDate: 'Sep 22 2023'
heroImage: '/website/miasmodeous-brute-force-search-repeat-node.png'
---

# Brute Force Search with bins

I'm working on a [project](https://github.com/ShouvikGhosh2048/miasmodeus) visualizing various reinforcement learning algorithms.

As a starting point, I have implemented brute force search.

## Brute force search
![Environment](/website/miasmodeous-environment.PNG)

The environment has a player moving through a world trying to reach a goal. The player has 3 actions - left, right and up - which can be played simultaneously, giving a total of 2 * 2 * 2 = 8 moves. Since the left and right actions cancel[^1], we can ignore moves with both left and right played, giving a total of 8 - 2 = 6 moves.

Let the start state represented as a node. For each of the six moves we can make from the start state, we add a node. For each of those nodes, we can add a node for each of the six moves we can make from there. This keeps repeating.

![Node tree](/website/miasmodeous-brute-force-search-tree.png)

We can search these nodes for possible solutions for the environment. To do this, we will use a queue.
- We add the start node to the queue.
- We remove the front node from the queue. For each of it's children, we consider the list of moves till the child and play that list of moves on the environment. If we get a solution, we are done. Else we add the child node to the queue. We repeat this step until we get a solution.

Using a queue this way results in the nodes being visited in layers, where each layer signifies the number of steps:

![Node tree](/website/miasmodeous-brute-force-search-layers.png)

Here's an example of brute force search.

<video controls width="100%">
    <source src="/website/brute-force-search.mkv"/>
</video>

## Optimizing brute force search
The issue with brute force search is that it can be slow. The start state has 6 children, those children have 6 * 6 children in total, and so on - the number of nodes increases exponentially with the number of layers explored.

One way to optimize this is to consider a bounding box around the world. If the player ends up outside the box after the moves of a node are played, we don't add the node to the queue. This prevents searching the state's descendants. Even after this optimization the search is slow. Also this won't always work - for example - this may not work on maps which require the player to fly in the air with a large vertical velocity - the player may end up escaping the bounding box.

## Binning

For this section, we are only going to consider worlds with a player, static objects and the goal.

Consider two instances of the environment where the player has the same position and same velocity. When we apply the same move in both of the environments, we should see the same results.

Thus during our brute force search, if we observe a node which has the same final position and velocity as a node which had been queued earlier, we don't need to queue this node - since the earlier queued node will end up giving the same descendants as this node, we don't need to additionaly explore this nodes descendants. Thus we could optimize our search by not queuing up this node.

### Binning by position and velocity

Now consider two nodes whose final positions and velocities are close. We could use the earlier idea here - if we see a node whose final position and velocity is close to a node which had already been queued, we don't queue up this node. **This is an approximation** unlike the earlier idea, but you're likely to skip unnecessary nodes as we skip on close matches. Since this is an approximation, this may not be able find a solution even if one exists.

![Node tree](/website/miasmodeous-brute-force-search-repeat-node.png)

I use this approximation for my optimization. I divide the possible positions and velocities into bins. While searching the nodes, before we queue a node we check whether there was ever a queued node in the bin corresponding to the node's final position and velocity. If not, we queue the node.

This results in the following search:
<video controls width="100%">
    <source src="/website/brute-force-with-position-and-velocity.mkv"/>
</video>

### Binning by position
This does result in a faster search, but the search is still slow. What if instead of binning by position and velocity, we just bin by position?

<video controls width="100%">
    <source src="/website/brute-force-with-position.mkv"/>
</video>

As you can see above, the search becomes faster without velocity.

### Binning + priority

Binning only by position however has it's issues.

<video controls width="100%">
    <source src="/website/brute-force-position-problem.mkv"/>
</video>

Here the solution is to travel left, then travel right and use the velocity gained to reach the other block.
This however requires revisiting positions, which binning by position doesn't allow.

So we want to consider velocity too. The position and velocity method explores more but is slower. The position method is faster but doesn't explore as much.

How about combining the two? 
- We could maintain two queues - queue1 and queue2. 
- Whenever we visit a node with a new position, we add the node to queue1.
- when we visit a node with an earlier visited position but a new velocity we add it to queue2.
- When choosing a node to explore further, we take a node from queue1 if it is non-empty, else we take it from queue2 if it is non-empty.

Thus we initially explore new positions, and when we don't have anymore positions to explore, we explore nodes with new velocities.

<video controls width="100%">
    <source src="/website/brute-force-with-position-then-velocity.mkv"/>
</video>

### Possible future explorations

Instead of using two queues, I could use a [priority queue](https://en.wikipedia.org/wiki/Priority_queue). I could explore different choices for node priority.

Currently the new velocity nodes searched in binning + priority includes nodes generated early in the the search which might lead to searching unnecessary early positions. Node priority could be used to make the search have a preference towards later generated nodes.

Also the binning currently doesn't consider dynamic blocks - the binning can be updated to consider dynamic blocks.

[^1]: The left and right actions don't exactly cancel due to the physics system, but this is generally a decent assumption and reduces the search space.