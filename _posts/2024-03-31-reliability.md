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

In distributed systems, it is commonly assumed that the network is fully connected, i.e., any two processes are connected through an undirected link.
However, there are some studies that consider **partially connected networks**.
In this post, we'll explore one such study.
In the next post, we'll focus on solving Byzantine reliable broadcast in partially connected networks.

#### Model
Consider an asynchronous distributed system composed of the set $\Pi$ of $n$ processes.
Assume that up to $f < n/3$ of the processes are Byzantine.
Each process has a unique ID, IDs are not necessarily consecutive, and it is infeasible for a Byzantine process to obtain additional IDs to launch a Sybil attack.
Each processes knows $n$, $f$, and the IDs of **all** processes; accordingly, the system is known (compare this kind of systems with unknown systems [[2]](https://www.di.fc.ul.pt/~bessani/publications/icdcs23-stellar-cup.pdf)[[3]](https://www.di.fc.ul.pt/~bessani/publications/tdsc16-bftcup.pdf)).

The processes are connected by a communication network represented by an undirected graph $G=(V,E)$ where each node represents a process, and each edge represents a communication link. 
Each process known the set of its neighbors.
Two processes can directly communicate with each other iff they are neighbors. 
Otherwise, processes have to rely on others to relay their messages and communicate. 
Intermediary nodes might be Byzantine, i.e., drop, modify, or inject messages. 
The communication links are reliable and authenticated. 
Processes **do not** have access to digital signatures.
<span style="color: green;font-weight: bold;">The communication network is at least $2f+1$ connected.</span>

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
If $j$ receives $\langle i, m \rangle$ from these processes, it 

#### Reference
1. [Practical Byzantine reliable broadcast on partially connected networks](https://arxiv.org/pdf/2104.03673.pdf), ICDCS, 2021
2. [On the minimal knowledge required for solving Stellar consensus](https://www.di.fc.ul.pt/~bessani/publications/icdcs23-stellar-cup.pdf), ICDCS, 2023
3. [Knowledge connectivity requirements for solving byzantine consensus with unknown participants](https://www.di.fc.ul.pt/~bessani/publications/tdsc16-bftcup.pdf), IEEE Transactions on Dependable and Secure Computing, 2016


