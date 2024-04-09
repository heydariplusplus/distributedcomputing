---
layout: post
title: From Synchrony to Asynchrony
# subtitle: View Synchronizer
# cover-img: /assets/img/path.jpg
# thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
tags: [Consensus]
# comments: true
# author: Hasan Heydari
---

In this post, we will review the definitions of well-known timing models in distributed computing and systems.

## Synchronous model 

## Partially synchronous model

#### General partially synchronous model
**Definition ([[1]](https://software.imdea.org/~gotsman/papers/bftlive-disc20.pdf)).** After GST, message delays between correct processes are bounded by a constant $\delta$; both GST and $\delta$ are *unknown*; before GST, messages can get arbitrarily delayed or *lost*; the processes are equipped with hardware clocks that can drift unboundedly from real time before GST, but do not drift thereafter.

#### Restricted partially synchronous model
**Definition ([[2]](https://arxiv.org/pdf/2301.09881.pdf)).** After GST, message delays between correct processes are bounded by a constant $\delta$; GST is unknown, but $\delta$ is *known*; before GST, messages can get arbitrarily delayed; the processes are equipped with hardware clocks that can drift unboundedly from real time before GST, but do not drift thereafter; if $t$ is the time when the first correct process begins the protocol execution, then at least $f$ other correct processes begin the protocol execution at most by time $t+\Gamma$, where $f$ denotes the number of Byzantine processes in the system and $\Gamma$ is a known bound.

## Asynchronous model

## References
1. [Making Byzantine Consensus Live](https://software.imdea.org/~gotsman/papers/bftlive-disc20.pdf), DISC, 2020
2. [Fever: Optimal Responsive View Synchronization](https://arxiv.org/pdf/2301.09881.pdf), OPODIS, 2023

