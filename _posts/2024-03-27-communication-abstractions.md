---
layout: post
title: A single protocol that spans three communication primitives
subtitle: Reliable communication, Byzantine consistent broadcast, and Byzantine reliable broadcast
# cover-img: /assets/img/path.jpg
# thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
tags: [Byzantine reliable broadcast, Byzantine consistent broadcast, Reliable communication]
# author: Hasan Heydari
---

In this post, we present a protocol that implements three communication primitives: Reliable communication, Byzantine consistent broadcast, and Byzantine reliable broadcast.
Before presenting the protocol, we first review the specification of these communication primitives.

#### Reliable communication

#### Byzantine consistent broadcast

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


#### Implementation

{% highlight pseudocode linenos %}
// Code for process i

echo = False
vote = False
reliableDeliver = False

upon broadcast(m)
   send <BROADCAST, m>ᵢ to all processes

upon receiving <BROADCAST, m>ⱼ from j 
   if echo == False
      trigger deliver(j, m)
      send <ECHO, <BROADCAST, m>ⱼ>ᵢ to all processes
      echo = True

upon receiving {<ECHO, <BROADCAST, m>ⱼ>ₖ | k ∈ Q} = M from a quorum Q
   if vote == False
      trigger consistentDeliver(j, m)
      send <VOTE, <BROADCAST, m>ⱼ>ᵢ to all processes
      vote = True

upon receiving {<VOTE, <BROADCAST, m>ⱼ>ₖ} from f+1 processes
   if vote == False
      trigger consistentDeliver(j, m)
      send <VOTE, <BROADCAST, m>ⱼ>ᵢ to all processes
      vote = True

upon receiving {<VOTE, <BROADCAST, m>ⱼ>ₖ | k ∈ Q} = M from a quorum Q
   if reliableDeliver == False
      trigger reliableDeliver(j, m)
      reliableDeliver = True
{% endhighlight %}