---
type: Paper Explanation
tags: []
society: AAAS
status: ongoing
publishD: 2022
---

# Author Affiliation
Anton Bakhtin, Noam Brown, Emily Dinan, Gabriele Farina ...
**Meta Fundamental AI Research Diplomacy Team**

DOI: [Paper Link](https://doi.org/10.1126/science.ade9097)

abbr1:: [[Abbreviation_1]] 

<br>

# Shortcomings

> [!summary]- AI agents to have an effective communication with humans 
> Major challenge of AI is to build system of AI agents that can plan, coordinate, negotiate with humans. Although we have some agents which posses same but they can't communicate with an intent or in belief of cooperating with others.
> 
> To have an effective negotiation, AI agents must model the beliefs, goals, intent and plan joint actions of other agents along with their own.
>  

# What have authors proposed?

- ***Cicero***, an AI agent that can negotiate in natural language in order to coordinate actions to cooperate or compete with other players of *strategy game diplomacy*. 

> [!question]- Why using *diplomacy* to train AI agents and not some other benchmarking?
> 
>> [!tldr]- Prior success of multi-agent learning were based on *Adversarial Environments* ! 
>> *Adversarial environments* such as AlphaGo for Go board game, Deep Blue for chess, Pluribus for multiplayer poker etc. These are the enviroments where communication has no value since all the available players are in competition with each other.  
>
>Even if there were some benchmarks where player can cooperate and compete at same time then its hard to reason about their actions
> 
> In Diplomacy,we need to cooperate and sometimes compete with other players in order to accomplish our goals. This makes this game as a challenging benchmark for multi agent learning.

# The game of Diplomacy

Strategy game of diplomacy is a board game where 2-7 player each representing a country of Europe as a diplomat coordinate to control supply centres (SC) which are strategic cities and provinces on a map. Each player move their units to defeat those of others to win possession of SC which evidently gives possesion to more units. 

At each turn, players engage in a private pairwise free-form dialogue with others during negotiation period and then all players simultaneously choose an action of one order per unit they control, units can either compete or cooperate.

Game may end when:-

- A player wins by controlling the maximum numbers of supply centres.

- All remaining players agree to a draw 

- Turns limit is reached, in which case score is determined by number of SCs' for that player.

# Challenges of Human-AI cooperation in Diplomacy

> [!summary]- Any Two player zero sum game can be solved via self-play with sufficient compute and model capacity
>> [!info]- Definition re **self-play**
>>  When in multi agent reinforcement learning, agents learn to improve their performance in certain game by playing against themselves.
>
> Chess, Go, Starcraft, Poker are all examples of two player zero sum (2p0s) games where reinforcement learning (RL) algorithms learn by *self-play*. 
> 
>> [!info]- Definition re **balanced games**
>> In balanced games, where players are expected to have approximately same amount of strentghs so no one dominates over another without competing and winning or losing over the use of their skills, 
>
> Self-play will converge to a policy which is expected to be unbeatable in balanced games

> [!summary]- Self-play will not work in games which involves cooperation among players
> Self-play has achieved superhuman performance in 2p0s games but when these agents are played against human players, they perform poorly due to learning a policy inconsistent with norms and expectations of potential human allies.
> 
> This brings us to the point where we need human data to make agent learn a policy which would be consistent with human norms and also is interpretable.

> [!summary]- Agent need to learn to build trust and even break it when it's goals are fulfilled in diplomacy, so it's a challenging domain
> At each turn, players engage in pairwise private dialogue which is mere verbal agreement and is not obligatory to be followed. This make the players to *trust* each other really difficult, and so agent must account for the risk that player may not stay true to their word.
> 
> An ability to reason about the goals, beliefs, intentions of others and persuade to build relationships by showing credibility are important factors that agent needs to learn.

