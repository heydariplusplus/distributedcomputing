---
layout: post
title: Communication Abstractions
# subtitle: Byzantine Reliable Broadcast on Partially Connected Networks
# cover-img: /assets/img/path.jpg
# thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
tags: [Byzantine reliable broadcast, Consistent broadcast, Reliable communication]
# author: Hasan Heydari
---

#### Byzantine reliable broadcast
Every instance of Byzantine reliable broadcast has a designated sender process $s$, who broadcasts a message $m$.
This abstraction provides two operations:
- $\mathtt{brbCast}(m)$ - through which $s$ broadcasts $m$.
- $\mathtt{brbDeliver}(m)$ - invoked by a receiver to deliver $m$ sent by $s$.

Any implementation of this abstraction must satisfy the following properties: 
- **Validity.** If the sender is correct and broadcasts a message $m$, then every correct process eventually delivers $m$.
- **No duplication.** Every correct process delivers at most one message.
- **Integrity.** If some correct process delivers a message $m$ with sender $s$ and process $s$ is correct, then $m$ was previously broadcast by $s$.
- **Consistency.** If some correct process delivers a message $m$ and another correct process delivers a message $m'$,then $m = m'$.
- **Totality.** If some message is delivered by any correct process, every correct process eventually delivers a message.