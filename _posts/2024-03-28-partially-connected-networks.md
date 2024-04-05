---
layout: post
title: Partially Connected Networks
# subtitle: Byzantine Reliable Broadcast on Partially Connected Networks
# cover-img: /assets/img/path.jpg
# thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
tags: [Byzantine reliable broadcast]
# author: Hasan Heydari
---

In distributed systems, it is commonly assumed that the network is **fully connected**, i.e., any two processes are connected through an undirected link.
However, there are some studies that consider **partially connected networks**.
In this post, we'll explore one such study presented in [[1]](https://www.cs.huji.ac.il/~dolev/pubs/unanimity-1981.pdf).

#### Model
Consider an asynchronous distributed system composed of the set $\Pi$ of $n$ processes.
Assume that up to $f < n/3$ of the processes are Byzantine.
A non-Byzantine process is said to be correct.
Each process has a unique ID, IDs are not necessarily consecutive, and it is infeasible for a Byzantine process to obtain additional IDs to launch a Sybil attack.
Each processes knows $n$, $f$, and the IDs of **all** processes; accordingly, the system is known (compare this kind of systems with unknown systems [[2]](https://www.di.fc.ul.pt/~bessani/publications/icdcs23-stellar-cup.pdf)[[3]](https://www.di.fc.ul.pt/~bessani/publications/tdsc16-bftcup.pdf)).

The processes are connected by a communication network represented by an undirected graph $G=(V,E)$ where each node represents a process, and each edge represents a communication link. 
Each process knows the set of its neighbors.
Two processes can directly communicate with each other iff they are neighbors. 
Otherwise, processes have to rely on others to relay their messages and communicate. 
Intermediary nodes might be Byzantine, i.e., drop, modify, or inject messages. 
The communication links are reliable and authenticated. 
Processes **do not** have access to digital signatures.

#### The most interesting result presented in [[1]](https://www.cs.huji.ac.il/~dolev/pubs/unanimity-1981.pdf)

> **Theorem.**
To solve [Byzantine reliable broadcast](/2024-03-27-communication-abstractions), the communication network must be at least $2f+1$ connected.

Before presenting the proof of the above theorem, we present a preliminary lemma.

> Lemma. Consider a distributed system composed of four processes $A, B, C$, and $D$, where $B$ is Byzantine and others are correct.
Solving Byzantine reliable broadcast in this system is impossible if processes are connected as follows:

![Lemma](/assets/img/my_img/ABCD.png){: style="display:block; margin-left:auto; margin-right:auto;" width="200" }

> **Proof of Lemma.** 
For the sake of contradiction, assume that there is a protocol $\mathcal{P}$ that implements the Byzantine reliable broadcast abstraction even in this system.
Recall that one of the properties that $\mathcal{P}$ must satisfy is Validity (i.e., if the sender is correct, then eventually all correct processes will deliver the sender's message.); besides, it must satisfy No duplication (i.e., every correct process delivers at most one message.)
Assume that process $A$ is the sender and broadcasts a message $m$.
Consequently, $C$ must deliver $m$.
However, assume that $C$ receives two messages: $m$ from $D$ and $m'$ from $B$; now $C$ cannot make a distinction between these messages; hence, it must either deliver both or none of them.
If it delivers both messages, No duplication is violated. 
In case it avoids delivering a message, Validity is violated.
Accordingly, $\mathcal{P}$ is not an implementation of the Byzantine reliable broadcast abstraction, which is a contradiction.

We are now ready to present the proof of the above-mentioned theorem.

> **Proof of Theorem.** 









<!-- 

#### Why is communication network at least $2f+1$ connected?
We need to define a problem:
- $send(j,m)$
- $deliver(i,m)$

- Integrity. Given two correct processes $i$ and $j$, if $j$ delivers a message $\langle i, j, m \rangle$, $i$ previously executed $send(j,m)$.
- Validity. Given two correct processes $i$ and $j$, if $i$ executes $send(j, m)$, $j$ eventually delivers message $\langle i, j, m \rangle$.

Suppose a process $j$ receives a message $\langle i, m \rangle$.
If $i$ and $j$ are neighbors, $j$ ensures that the message was sent by $i$ due existing an authenticated link between $i$ and $j$.
However, if $i$ and $j$ are not neighbors, when $j$ receives $\langle i, m \rangle$, it is challenging to ensure this message was really sent by $i$.
The challenge arises when $j$ has a Byzantine neighbor $k$, and $k$ sends $\langle i, m \rangle$ to $j$.
Even if $j$ receives $\langle i, m \rangle$ from $f$ neighbors, it cannot ensure whether $\langle i, m \rangle$ was sent by $i$ as it might have $f$ Byzantine neighbors.
Now this question arises: knowing that there is at least one correct process in any set of $f+1$ processes, after receiving a message $\langle i, m \rangle$ from $f+1$ neighbors, can $j$ ensure whether $\langle i, m \rangle$ was sent by $i$?
Unfortunately, no (suppose $j$ receives $\langle i, m \rangle$ from $f+1$ neighbors, all of whom have a common Byzantine neighbor that sent this message to them.)

**Theorem.** Given two non-neighbor processes $i$ and $j$, after receiving a message $\langle i, m \rangle$, $j$ can ensure the message was sent by $i$ if it is received from at least $f+1$ node-disjoint paths starting from $i$ and ending at $j$.   
**Proof.** For the sake of contradiction, assume that $j$ can ensure that a received message $\langle i, m \rangle$ was sent by $i$ if it is received from $f$ node-disjoint paths.  
Consider the following two scenarios:
1. $j$ has only $f$ correct neighbors, all of whom are neighbors of $i$.
   If $j$ receives $\langle i, m \rangle$ from these processes, $j$ ensures that the message was sent by $i$.
2. $j$ has $f$ Byzantine neighbors.
These processes can pretend $i$ is their neighbor.
They can also pretend they have received a message $\langle i, m \rangle$ from $i$.
If $j$ receives $\langle i, m \rangle$ from these processes, it  -->


#### Reference
<!-- 1. [Practical Byzantine reliable broadcast on partially connected networks](https://arxiv.org/pdf/2104.03673.pdf), ICDCS, 2021 -->
1. [Unanimity in an unknown and unreliable environment](https://www.cs.huji.ac.il/~dolev/pubs/unanimity-1981.pdf), FOCS, 1981
2. [On the minimal knowledge required for solving Stellar consensus](https://www.di.fc.ul.pt/~bessani/publications/icdcs23-stellar-cup.pdf), ICDCS, 2023
3. [Knowledge connectivity requirements for solving byzantine consensus with unknown participants](https://www.di.fc.ul.pt/~bessani/publications/tdsc16-bftcup.pdf), IEEE Transactions on Dependable and Secure Computing, 2016


