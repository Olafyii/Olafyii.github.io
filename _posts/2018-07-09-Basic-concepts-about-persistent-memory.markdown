---
layout: post
title: Basic concepts about persistent memory
date: 2018-07-09 11:14:00 +0800
catalog: true
header-style: text
tags:
    - persistent memory
---

This post is a sketch of [persistent memory wikipedia page](https://en.wikipedia.org/wiki/Persistent_memory) and [this Youtube video](https://www.youtube.com/watch?v=E2KYqdyZcQY&list=PLg-UKERBljNztvB495CD6Ij9wBXh2tRTT).

- "In computer science, **persistence** refers to the characteristic of state that outlives the process that created it. "---Wikipedia
- "In computer science, **persistent memory** is any method or apparatus for efficiently storing data structures such that they can continue to be accessed using memory instructions or memory APIs even after the end of the process that created or last modified them."---Wikipedia
- Persistent memory is closely linked to the concept of persistence in its emphasis on program state that exists outside the fault zone of the process that created it. Thus **persistent memory has more fault tolerance**.
- Persistent memory is corresponding to **volatile memory**(only maintains its data while the device is powered).
- Memory-like access is the defining characteristic of persistent memory. It can be provided using microprocessor memory instructions, such as load and store.
- Persistent memory extends beyond non-volatility of stored bits. **It is kind of a more abstract forms of computer storage, such as file systems.**
- P. Mehra and S. Fineberg, "**Fast and flexible persistence: the magic potion for fault-tolerance, scalability and performance in online data stores**" 18th International Parallel and Distributed Processing Symposium, 2004. Proceedings., Santa Fe, NM, USA, 2004, pp. 206-. doi: 10.1109/IPDPS.2004.1303232
- Persistent memory is based on 3D XPoint technology.
- Persistent memory allow products have the attributes of both storage and memory.
  - It is **persistent** like storage---they can hold their contents across power cycles.
  - They are **byte addressable**, like memory. Programs can access data structures in place.
- Persistent memory is fast enough to access directly from the processor **without stopping to do the Block I/O** required for traditional storage. (Doing block I/O means that the application or file system is sending blocks to the disk drive to be written or asking for blocks using a logicalblock address (LBA).)

![img](\assets\images\screenshot63.png)

![img](\assets\images\screenshot64.png)

![img](\assets\images\screenshot65.png)

![img](\assets\images\screenshot66.png)