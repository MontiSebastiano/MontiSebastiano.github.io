---
layout: post
title: SokoBench - Evaluating Long-Horizon Planning and Reasoning in Large Language Models
date: 2026-01-28 16:00:00-0400
inline: false
related_posts: false
---

# Can Reasoning Models Actually Plan? A study on Sokoban linear corridors

As we continue to push the boundaries of Large Reasoning Models (LRMs), there is a common assumption that allocating more thinking tokens automatically translates to better planning. 
However, our latest research suggests there is a "wall" that even the most advanced models are hitting. 

In our new paper, **SokoBench**, we evaluated the long-horizon planning capabilities of state-of-the-art models using the classic puzzle game Sokoban. 
In this game, a player must navigate a two-dimensional, spatially constrained environment to push boxes onto predefined goal locations.

## The "Strawberry" Problem of Spatial Planning

To truly understand where planning breaks down, we moved away from complex maps and multi-box puzzles. 
Instead, we designed a benchmark using **simplified Sokoban puzzles** consisting of vertical and horizontal linear corridors of variable lengths. 
The player and the goal are positioned at opposite ends, with a single box placed in the middle. 

This design isolates long-horizon planning from environmental complexity: the models simply need to move the player in a single direction for a specific number of steps to push the box onto the goal.
If these models can reason on complex logic puzzles, they should easily be able to count the steps in a 1D corridor and perform the correct number of moves in the correct direction, right?

Not exactly.

## The 25-Move Wall

Our findings revealed a consistent and sharp degradation in performance as soon as a solution required more than **25-30 moves**. 
Even though the task is trivial for a human, LRMs exhibit a "counting drift" where they lose track of the internal state.

We observed two distinct failure modes:
- **Wandering**: Instead of following a systematic plan, models often "wander" within the solution space, repeating the same actions or reasoning steps until they reach their token limit.
- **Invalid Exploration**: Models sometimes walk through walls, showing that their internal representation of the game's physics is structurally incoherent.

## Can External Tools Fix It?

We also tested an **LLM-Modulo** framework, equipping models with external **PDDL** (Planning Domain Definition Language) parsers, verifiers and solvers. 
While this eliminated the risk of performing invalid moves, it still did not solve the underlying issue.

The models struggled to accurately translate the Sokoban corridors into correct PDDL problems due to their limited counting abilities.
Even with PDDL tools, the performance boost was modest; particularly in **vertical corridors**, which seemed to be more challenging compared to horizontal ones.

## Why This Matters

Our study suggests that current failures in spatial planning aren't just a matter of complexity; they are about a **deficiency in basic symbolic operations** like counting and state-tracking. 

We believe these are architectural limitations that "test-time scaling" (giving the model more time to think) might not overcome on its own.
True spatial intelligence requires more than just wandering through a solution space; it requires a grounded, consistent representation of the world. 
As we look toward the future, our work highlights the need for architectural innovations that provide better symbolic grounding for these reasoning giants.