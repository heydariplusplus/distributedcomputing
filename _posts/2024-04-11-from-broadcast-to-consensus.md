---
layout: post
title: From Broadcast to Consensus
# subtitle: A Communication Primitive to Form Quorums
# cover-img: /assets/img/path.jpg
# thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
tags: [Consensus, Broadcast, Synchronizer]
# author: Hasan Heydari
---

In this post, we design a consensus protocol based on a broadcast protocol and a view synchronizer protocol.

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



{% highlight pseudocode linenos %}
// Code for process i

upon newView(v)
   currView = v
   voted = False
   send <NewLeader, curView, preparedView, preparedVal, cert>ᵢ to leader(v)

upon receiving {<NewLeader, v, viewⱼ, valⱼ, certⱼ>ⱼ | j ∈ Q} = M from a quorum Q
   pre: currView == v ∧ i = leader(v) ∧ (∀ m ∈ M: validNewLeader(m))
   if ∃ j: viewⱼ = max{viewₖ | k ∈ Q} != 0
      Brb_cast(<Propose, v, valⱼ, M>ᵢ)
   else 
      broadcast(<Propose, v, myVal(), M>ᵢ)

upon consistentDeliver(j, <Propose, v, val, M>ⱼ = m)
   pre: currView == v ∧ voted = False ∧ safeProposal(m)
   currVal = val
   voted = True
   send <Propose, v, myVal(), M>ᵢ

{% endhighlight %}
