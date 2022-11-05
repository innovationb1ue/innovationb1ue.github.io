---
layout: raft
title: Raft Consensus Alglorithm
date: 2022-11-03 11:07:16
tags: 
- Raft
typora-root-url: ./Algorithm
---

Link to the algorithm paper: [The Paper](https://raft.github.io/raft.pdf)

![](./image-20221103111102300.png)

Learn raft through this blog

https://github.com/eliben/raft

ConsensusModule is the core raft struct (**CM**). It forms a basic Node in raft. 

##### What problem does this solve?

Disaster tolerance in the distributed system such as Redis sentinel mode. Whenever a node fails, another node becomes the master and provides consistent data. In the price that a write command now has to be replicated to all the nodes in the cluster and only after the majority of nodes save that the command, we can say that command is "committed". 

##### How it works

Only the leader can save new logs. Once the leader receives a command, it replicated that to all his followers. Each follower maintains a timer which will lead to the election and only the one with the most consistent log with the majority should win the election and become a new leader. 

##### How a client finds a leader? 

peer ID and RPCs are used to communicate replicas and leaders. 

##### Nodes Start Process

1. Initialize Nodes (n = 3)
2. start the election timers all together
3. one will become the leader and add 1 to the term. Others become followers. 
4. Start handling submits. 

##### RPC round-trips to commit a new command 

Simple answer: 2 

**Q:** How many RPC round-trips does it take to commit a new command?

**A:** Two. The first round is for the leader to send the next log entries to followers and for the followers to acknowledge them. When the leader processes a reply to an AE, it may update its commit index based on the response. The second round will send an updated commit index to followers, who will then mark these entries as committed and will send them on the commit channel. As an exercise, go back to the code snippets above and find the pieces of code that implement these steps.

##### Election safety

When a runaway server reconnects to the cluster, it might come back with a higher term and start an election. 

So the normal leader in the cluster will become its follower but the comeback server won't win the election due to failure in the completeness check. A winner has to win the majority of votes in the cluster with a higher term (satisfied) and complete more logs (not satisfied). 

This is because when a server disconnected from the cluster, it won't be able to get new logs anymore since it won't win the election and become a leader. It starts self-spinning and gets higher terms but no more logs. 

##### Persistence

Persistence states in the figure should be flushed with every write. This could be optimized. 
