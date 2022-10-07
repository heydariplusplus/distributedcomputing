---
layout: post
title: "Consensus Number of Atomic Storage"
categories: concurrency
tags: concurrency consensus-number atomic-storage
---

<div class="thm-box" markdown="1">
<div class="box-title" markdown="1">
**Theorem** 
</div>
<div class="box-content" markdown="1">
Read/write atomic registers have consensus number one.
</div>
</div>

<div class="thm-box" markdown="1">
<div class="box-title" markdown="1">
**Proof Outline** 
</div>
<div class="box-content" markdown="1">
1. We show that there exists a bivalent initial state. 
2. From a bivalent state, we show the system reaches another bivalent state.

From these two cases, we can conclude that the system might enter a loop. 
If it is the case, the system stays in the loop forever. 
</div>
</div>






