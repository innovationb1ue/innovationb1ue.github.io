---
title: DB-Storage
catalog: true
date: 2023-01-26 11:02:01
subtitle: CMU15445-2022FALL
header-img:
tags:
categories: Learn
typora-root-url: ./DB-Storage
---

## Storage

Db is organized in pages. 

Heap file

![image-20230126110313415](./DB-Storage/image-20230126110313415.png)



## Page Layout

Page is like a table. For most DBs a single page will only hold a single table. But it can hold more than one. 

![image-20230126110442721](./DB-Storage/image-20230126110442721.png)Oracle require pages to be self-contained.  Each page will  also store the schema and talbe info. 



But how do we store tuples in a page?

![image-20230126110828232](./DB-Storage/image-20230126110828232.png)

* Strawman Idea.  But you have to do linear search to find the tuple. **Super Slow**



**Slotted Pages (Most used)**

![image-20230126111124158](./DB-Storage/image-20230126111124158.png)



Slot array maps slots to tuples. 

The header keeps track of the num of used slots and the starting location of the last slot used. 



**Record IDs**

Track individual tuples. 

In postgres, it is named `ctid` 



## Tuple Layout

* Tuple is a flow of bytes. DBMS interpret them into structures. 
* ![image-20230126113012320](./DB-Storage/image-20230126113012320.png)
* Denormalized tuple: pre-join related tuples and store them together in the same page. 

![image-20230126113532867](./DB-Storage/image-20230126113532867.png)

