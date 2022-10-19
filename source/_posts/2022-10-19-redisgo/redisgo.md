---
title: RedisGo - 一个Go编写的高性能Redis服务器
catalog: true
date: 2022-10-19 18:21:21
subtitle:
header-img:
tags:
categories:
---

#### Intro

Project Redisgo [Github](https://www.github.com/innovationb1ue/redisgo)

一个使用Go编写的Redis Server服务端。完全实现Redis serialization protocol (RESP) specification。保证了支持任何语言规范的Client。性能达到原C-Redis的80%-95% (根据命令与平台不同可能会有所差异)

#### Code base

改编自[ThinRedis](https://github.com/VincentFF/thinredis)，原作者实现了一部分基础命令的RESP。RedisGO从[这个](https://github.com/VincentFF/thinredis/tree/86fa648426da7e9c3ff4c04aef1e43f1fdc7b1ac)版本开始进行优化与功能添加，优化了登陆处理与命令执行逻辑, 并且添加了更多的数据结构与命令支持。



#### Benchmark


Benchmark result is based on [redis-benchmark](https://redis.io/topics/benchmarks) tool.  
Testing on MacBook Pro 2021 with M1 pro, 16.0 GB RAM, and on macOS Monterey.

The first one is RedisGO result and the second is from Redis.  
Note that this result could vary tremendously. Generally we say we reach 80-90% of the original C Redis performance. 
`benchmark -c 50 -n 200000 -t [get|set|...] -q`

```text
SET: 176678.45   requests per second, p50=0.143 msec                    
GET: 187969.92 requests per second, p50=0.151 msec                    
INCR: 186741.36 requests per second, p50=0.135 msec                    
LPUSH: 173611.12 requests per second, p50=0.143 msec                    
RPUSH: 161943.31 requests per second, p50=0.143 msec                    
LPOP: 187265.92 requests per second, p50=0.135 msec                    
RPOP: 186915.88 requests per second, p50=0.135 msec                    
SADD: 186915.88 requests per second, p50=0.135 msec                    
HSET: 185873.61 requests per second, p50=0.143 msec                    
SPOP: 188501.42 requests per second, p50=0.135 msec                    
MSET (10 keys): 139275.77 requests per second, p50=0.199 msec    
```

```text
SET: 185873.61 requests per second, p50=0.135 msec                    
GET: 185528.77 requests per second, p50=0.127 msec                    
INCR: 183992.64 requests per second, p50=0.135 msec                    
LPUSH: 205338.81 requests per second, p50=0.135 msec                    
RPUSH: 208116.55 requests per second, p50=0.135 msec                    
LPOP: 197238.64 requests per second, p50=0.135 msec                    
RPOP: 197628.47 requests per second, p50=0.135 msec                    
SADD: 201409.88 requests per second, p50=0.135 msec                    
HSET: 201409.88 requests per second, p50=0.135 msec                    
SPOP: 212765.95 requests per second, p50=0.127 msec                    
MSET (10 keys): 181323.66 requests per second, p50=0.199 msec 
```

#### Support Commands
All commands used as [redis commands](https://redis.io/commands/). You can use any redis client to communicate with RedisGO.

| key     | string      | list   | set         | hash         | channels  | sorted set |
| ------- | ----------- | ------ | ----------- | ------------ | --------- | ---------- |
| del     | set         | llen   | sadd        | hdel         | subscribe | zadd       |
| exists  | get         | lindex | scard       | hexists      | publish   | zrange*    |
| keys    | getrange    | lpos   | sdiff       | hget         |           |            |
| expire  | setrange    | lpop   | sdirrstore  | hgetall      |           |            |
| persist | mget        | rpop   | sinter      | hincrby      |           |            |
| ttl     | mset        | lpush  | sinterstore | hincrbyfloat |           |            |
| type    | setex       | lpushx | sismember   | hkeys        |           |            |
| rename  | setnx       | rpush  | smembers    | hlen         |           |            |
|         | strlen      | rpushx | smove       | hmget        |           |            |
|         | incr        | lset   | spop        | hset         |           |            |
|         | incrby      | lrem   | srandmember | hsetnx       |           |            |
|         | decr        | ltrim  | srem        | hvals        |           |            |
|         | decrby      | lrange | sunion      | hstrlen      |           |            |
|         | incrbyfloat | lmove  | sunionstore | hrandfield   |           |            |
|         | append      | blpop  |             |              |           |            |
|         |             | brpop  |             |              |           |            |

*means partially implemented or is being worked on. 



#### TODO

* 实现所有的Redis命令。目标接近100%原Redis性能，在部分命令上实现超越性能。
* 精细的内存管理。原Redis一旦分配内存将[不会主动释放内存](https://redis.io/docs/reference/optimization/memory-optimization/)给OS。得益于Go的自动GC（Garbage colleciton），我们只要注意不要编写内存泄漏的代码,GC会代替我们清理所有的垃圾。在Key被销毁后，RedisGO占用的内存将会被释放，系统中的其他应用将有机会使用更多的内存。这在原Redis中是不可能的。
