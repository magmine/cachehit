---
layout: post
title:  "System Design 101"
date:   2021-11-03 16:10:28 +0100
categories: Computer-science
desc: ""
---

## Desing consistent hashing

### Problem
- To achieve horizontal scaling, we need to distibute requests / data efficiently and as evenly as possible accross multiple servers
- An easy and commong way to do that is by using this this method of hashing:
    - hash(key) % N, where N is the number of servers, key is a unique identifier of request/data
    - The result is good if the hash function is uniform enough
- The problem with the latest hash function is when we add more servers or some of the go down, in the mapping between the request / data and the server that process it is shuffled a lot
- If data is cached in servers we will hit a lot of cache misses degrading the performance of our app

### Solution is consistent hashing

#### What is consistent hashing
Consistent hashing is a special kind of hashing sush that when a hash table is re-sized only `K/N` elements are reshufled (`N` the size of Htable and `k` the number of keys)

#### Consistent hashing basic approach
- We choose a hash function `f` where `f(key) = V` where `V £ [0 - n]`, (example f = SHA-1 => [0 - 2^160 - 1])
- We hash a unique identifier of our servers (their name / ip /...)
- We hash the identifier of request / data
- The request should be mapped to an available server with the hash that is first encountred in clockwise direction

#### Basic approach problem

There are two main problems with the basic approach:

- It is impossible to keep the same size of server partitions when servers are added or removed, some of them can become very small others very large
- Non uniform key-distribution on the ring, resulting on very small and very big partitions

#### Consistent hashing advanced approach

- Use virtual nodes, each server whill have multiple hashes, and thus will serve keys over multiple partitions
- Studies showed that the bigger the number of virtual nodes the better partitioning
- The problem is it requires more memory, thus needs to be taken as desing desicion