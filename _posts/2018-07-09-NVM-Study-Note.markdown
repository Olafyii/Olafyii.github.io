---
layout: post
title: NVM Library Study Notes
date: 2018-07-09 11:14:00 +0800
categories: persistent memory
---

This post is mainly my understanding about [this article](http://pmem.io/2014/09/01/nvm-library-overview.html).

 

Prior knowledge:

- "In computer science, persistence refers to the characteristic of state that outlives the process that created it. "---Wikipedia
- "In computer science, persistent memory is any method or apparatus for efficiently storing data structures such that they can continue to be accessed using memory instructions or memory APIs even after the end of the process that created or last modified them."---Wikipedia
- Persistent memory is corresponding to volatile memory(only maintains its data while the device is powered).
- Memory-like access is the defining characteristic of persistent memory. It can be provided using microprocessor memory instructions, such as load and store.
- Persistent memory extends beyond non-volatility of stored bits. It is kind of a more abstract forms of computer storage, such as file systems.

 

The overall library architecture:

![img](/assets/images/libarch.jpg)

NVM library provides a pmem-aware malloc() function. Because the traditional malloc() and free() do not comprehend the idea of persistence. e.g. If a program allocates a blob of memory using malloc(), but dies before linking anything to it, that memory is a persistent memory leak and the pool is then inconsistent from that point on. With volatile memory, that's not an issue since it starts from nothing each time the program runs.

NVM library not only works on top of persistent memory, but also on any non-volatile memory.