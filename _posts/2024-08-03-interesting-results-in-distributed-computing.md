---
layout: post
title: Interesting Results in Distributed Computing and Systems
# subtitle: A Communication Primitive to Form Quorums
# cover-img: /assets/img/path.jpg
# thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
# tags: []
# author: Hasan Heydari
---

# CFT and BFT

{: .box-note}
**Transforming a Deterministic CFT algorithm to a BFT algorithm.**
Let $A$ be any deterministic distributed algorithm designed for non-synchronous systems that tolerates crash faults.
The following two mechanisms are necessary and sufficient to transform $A$ to an algorithm that tolerates Byzantine faults without increasing the number of processes:
(1) a mechanism to prevent equivocation, and (2) a mechanism to ensure the transferable authentication of network messages (e.g., using digital signatures)
[[ref]](https://dl.acm.org/doi/10.1145/2332432.2332490).