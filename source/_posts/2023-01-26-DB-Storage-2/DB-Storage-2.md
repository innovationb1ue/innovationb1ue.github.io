---
title: DB-Storage-2
catalog: true
date: 2023-01-26 11:47:56
subtitle:
header-img:
tags:
categories:
- Learn
- DB
---

#### Disk-Oriented Arch

DBMS handle the flow between volatile memory and non-volatile disk. 

#### Page-Oriented Arch

![image-20230126115026955](./DB-Storage-2/image-20230126115026955.png)



Think: what are the problems with slotted page design. 

- **Fragmentation**. Delete tuples won't vaccum the space they take and sort them. 
- **Useless Disk I/O.** We need to bring entire page in when we want only a tuple inside. 
- **Random Disk I/O.**  (Update 20 tuples in 20 pages, read them all in and write them back out)

