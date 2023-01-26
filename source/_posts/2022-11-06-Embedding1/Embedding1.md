---
layout: 
title: embedding raft into redisGO
date: 2022-11-06 18:52:38
tags: 
 - RedisGO
 - GO
 - Redis
---

##### Example process

1. propose channel: we create a `chan string` to propose string commands to the Raft layer. 
2. confChange channel: indicate the Rate node to restart and apply the new configuration. 





##### Refactor

* Parse cluster configurations
* Pass client commands to Raft layer. 
  * send to propose channel
  * wait for the command to be committed by leader
  * return command response
* listen to commit channel and forward Raft layer commited commands to DB store. 



##### Command Callback

* propose a command
* the command should include a callback channel. 
* The proposer listen to the callback channel
* commit writer finds the callback and returns the execute result to that channel 
