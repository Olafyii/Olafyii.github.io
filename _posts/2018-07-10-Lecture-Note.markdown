---
layout: post
title: '"On the improvement of Performance and Endurance of Crossbar Resistive Memory"---lecture notes'
date: 2018-07-10 09:22:00 +0800
categories: persistent_memory
---



Background:

University of Pittsburgh

Cathedral of learning 

CS@SCI---"school of computing and information" 

 

What they are doing:

- NVM based memory system
  - **ReRAM based main memory system---Today's topic**
- Supporting Path-ORAM in the cloud (privacy issue)
  - Privacy issue. e.g. Dropbox is not allowed in China. Medication information shound not be put on cloud.
  - Intel SGX. (Curious server/server not trusted)
- CNN-acceleration
- NVM/DRAM based accelerators

 

![img](/assets/images/MVIMG_20180710_094733.jpg)

Tradition: "1T1R" Structure (Transistor size much bigger than cell---not good)

Crossbar structure ("0T1R", "1D1R")

disadvantage: Sneak currents, IR drop

IR drop: hurt performance and endurance

![img](/assets/images/MVIMG_20180710_095104.jpg)
$$
t*e^{k\times Vd}=C
$$
t---Reset latency

Vd is small due to sneak current, t wil be larger.

Optimize t according to the number of "1"(low resistance)

More LRS cells---larger IR drop---longer time(for LRS cells, sneak current is larger, thus Vd is smaller, according to the formula, t is larger)

The impact diminishes as row become closer to write driver.

Use the number of LRS cells(worst case) in a bitline to optimize RESET time.

Count the number of "1" by "current accumulation effect"![img](/assets/images/MVIMG_20180710_100838.jpg).

![img](/assets/images/MVIMG_20180710_101228.jpg)

Reduce LRS cells

![img](/assets/images/MVIMG_20180710_101909.jpg)

 

Endurance~$(\dfrac{tw}{t0})^c$

tw---write latency

t0, c---constant

![img](/assets/images/MVIMG_20180710_103442.jpg)

Efficient Write

EW=$\|(\dfrac{tL}{t})^2\|$

longest duration tL to write.

Endurance not only related to how many writes.

Write time depends on worst case. Some bitlines with more HRS cell do not need such long time. This will reduce its endurance.

Solution---remap.

 

Suggestion on writing paper: performance is not the most important, idea is important.

Currently, ReRAM is too expensive for storage.