---
layout: post
title: "Solving Consensus in Permissionless Settings I"
categories: consensus
# katex: True
---
In this series of two posts, we want to take a look at "solving consensus in permission-less settings".
Let's review the definition of consensus.

**Definition (consensus)** Given a distributed system composed of $$n \geq 2$$ processes, any algorithm
that solves consensus must satisfy three properties:
- Agreement: **all** correct processes must decide the same value.
- Validity: if **all** correct processes propose the same value $$v$$, they must decide $$v$$.
- Termination: **all** correct processes must decide eventually.

In a nutshell, the meaning of "correct" depends on the failure model.
Here, we consider the most general failure model, i.e., [Byzantine failures](https://en.wikipedia.org/wiki/Byzantine_fault){:target="_blank"}.
If a process is not Byzantine, it is *correct*!
Notice that **all** correct processes must decide!
In a closed environment, where the set of processes is known in advance, **all** is meaningful.
However, in a permission-less setting, where processes can join and leave the system at any time, i.e., the set of processes is not fixed, **all** is not meaningful!
Consequently, the following question is immediate:
> How can we ensure that **all** correct processes satisfy the properties of consensus in a permission-less setting?

It is clear that we must consider some assumptions in order to answer the above question.
In this post, we present one of those assumptions, and we'll present other assumptions in upcoming posts.

**Assumption 1.** The communication model is synchronous and $$f$$, the number of faulty processes, is known.

Before discussing how we can answer the above question using Assumption 1, let's see why this assumption is required.

**Theorem 1.** Solving consensus when the number of processes, $$n$$, is unknown is impossible in [partially synchronous systems](https://decentralizedthoughts.github.io/2019-09-14-flavours-of-partial-synchrony/){:target="_blank"}, even when there is no faulty process and the system is static!

When we say that $$n$$ is unknown, it means that an algorithm designer who wants to design an algorithm to solve consensus cannot use a pre-defined $$n$$ in his/her algorithms.
Besides, when we say that the system is static, it means that the set of processes does not change over time.
Now, let's see a sketch of the proof.

**Proof.** For the sake of contradiction, assume that it is possible to solve consensus when $$n$$ is unknown in partially synchronous systems.
Consider the following worlds:
1. (World I)   $$\Pi_p = \{p_1,p_2,\dots,p_k\}$$, and the communication delay between these processes is negligible. Also, assume that the proposal of each process is 0. Because of the validity property, the decided value must be 0. Assume that processes decide a value before $$\delta_p$$.
2. (World II)  $$\Pi_q = \{q_1,q_2,\dots,q_k\}$$, and the communication delay between these processes is negligible. Also, assume that the proposal of each process is 1. Because of the validity property, the decided value must be 1. Assume that processes decide a value before $$\delta_q$$.
3. (World III) $$\Pi = \Pi_p \cup \Pi_q$$, and $$\Pi_p$$ (resp. $$\Pi_q$$) has the properties of World I (resp. World II). Assume that there is a [network partition](https://en.wikipedia.org/wiki/Network_partition){:target="_blank"} between $$\Pi_p$$ and $$\Pi_q$$ that will be resolved after $$\max(\delta_p,\delta_q)$$.

Members of $$\Pi_p$$ (resp. $$\Pi_q$$) cannot distinguish World I from World III (resp. World II from World III).
Consequently, members of $$\Pi_p$$ (resp. $$\Pi_q$$) decide 0 (resp. 1) in World III as well.
That is a contraction, because the agreement property is violated!
$$\square$$


It follows from Theorem 1 that we must consider a more strict communication model than partially synchrony for solving consensus with unknown number of processes.
This is why we assumed that the communication model must be synchronous in Assumption 1.
In the next post, we discuss why we need to known the number of faulty processes.

