---
layout: post
title: Modular Design of Distributed Protocols 
subtitle: View Synchronizer
# cover-img: /assets/img/path.jpg
# thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
tags: [Consensus, Synchronizer]
# author: Hasan Heydari
---

Modularity is a strategy to enable the decomposition of a system into cohesive, reusable, and independent modules, facilitating simplicity, reusability, scalability, collaboration, testing, debugging, and abstraction.
In this post, we will delve into the *view synchronizer*, an abstraction that can be employed in designing modular distributed protocols.
We start by presenting a brief description of the abstraction.

## View Synchronizer
Partially synchronous Byzantine consensus protocols typically structure their execution into a sequence of *views*, each with a designated *leader* responsible for
driving the protocol towards a decision.
If a view does not reach a decision (e.g., because its leader is faulty), processes switch to the next one.
To ensure liveness, the protocol needs to guarantee that all correct processes will eventually enter the same view with a correct leader and stay there long enough to complete the communication required for a decision.
Achieving such view synchronization is nontrivial, because before GST, clocks that could measure the duration of a view can diverge, and messages that could be used to bring processes into the same view can get lost or delayed. Thus, by GST processes may end up in wildly different views, and the protocol has to bring them back together, despite any disruption caused by Byzantine processes.
Some of the Byzantine consensus protocols (e.g., [PBFT](https://pmg.csail.mit.edu/papers/osdi99.pdf)) integrate the functionality required for view synchronization with the core consensus protocol, which complicates their design. 
In contrast, some other work suggest separating the complex functionality required for view synchronization into a distinct component – *view synchronizer*, or simply *synchronizer*. 
This approach allows the modular design of Byzantine protocols, with mechanisms for ensuring liveness reused among different protocols.

A simple and precise specification of a synchronizer abstraction sufficient for single-shot consensus is presented in [[1]](https://software.imdea.org/~gotsman/papers/bftlive-disc20.pdf).
The specification ensures that from some point on after GST, all correct processes go through the same sequence of views, overlapping for some time in each one of them.
It precisely characterizes the duration of the overlap and gives bounds on how quickly correct processes switch between views.
Besides, it proposes a synchronizer implementation, called <span style="font-variant:small-caps;">FastSync</span>.

#### System Model
1. The system is composed of $n=3f+1$ processes, out of which at most $f$ can be Byzantine.
2. Timing model: general partial synchrony (i.e., after GST, message delays between correct processes are bounded by a constant $\delta$; both GST and $\delta$ are unknown; before GST, messages can get arbitrarily delayed or lost; the processes are equipped with hardware clocks that can drift unboundedly from real time before GST, but do not drift thereafter).
   It is worth noting that the partial synchrony model has another [restricted definition](https://decentralizedthoughts.github.io/2019-09-13-flavours-of-partial-synchrony/): GST occurs after some finite time; there is no bound on message delays before GST, but after GST all message must arrive within some known bound $\Delta$.
3. Processes do not use digital signatures, relying only on authenticated point-to-point links.

#### Synchronizer's functions and properties
###### Functions
The synchronizer provides two functions:
- $\mathtt{newView}(v)$ - the job of the synchronizer is to produce this notification at each correct process, telling it to enter view $v$.
- $\mathtt{start}()$ - a process starts the synchronizer by calling this function.

###### Notations
- $\Pi$ denotes the set of processes.
- $View = \\{1,2,\dots \\}$ denotes the set of valid views; $0$ denotes an invalid view.
- $Time$ denotes the set of time points.
- $F: View \cup \\{0\\} \rightarrow Time$. All correct processes overlap in each view $v$ for
a duration determined by $F(v)$.
- $E_i(v)$ is the time when a correct process $i$ enters view $v$.
- $E_{first}(v)$ and $E_{last}(v)$ denote respectively the earliest and the latest time when some correct process enters $v$.
- $S_{first}$ and $S_{last}$ denote respectively the earliest and the latest time when some correct process calls $\mathtt{start}()$.
- $S_k$ denotes the earliest time by which $k$ correct processes call $\mathtt{start}()$.
- $V$ denotes the view from which the synchronization starts.
- $GV(t)$ is the maximum view entered by a correct process at or before $t$, or $0$ if not view was entered by a correct process. 

###### Properties
Any implementation of the synchronizer abstraction satisfies the following properties:
1. Views may only increase at a given process $i$. 
   Formally, $\forall i \in \Pi, \forall v, v' \in View. (E_i(v) \text{ and } E_i(v') \text{ are defined}) \land v<v' \Rightarrow E_i(v)<E_i(v')$. 
2. The view synchronization starts from some view $V$, entered after GST.
   Formally, $E_{first}(V) \geq GST$.
3. Starting from $V$, correct processes do not skip any views.
   Formally, $\forall i \in \Pi, \forall v \geq V.$ $i$ is correct $\Rightarrow$ $i$ enters $v$.
4. Starting from $V$, correct processes enter each view $v \geq V$ within at most $d$ of each other.
   Formally, $forall v \geq V. E_{last}(v)\leq E_{first}(v)+d$. 
5. Starting from $V$, correct processes enter each view $v \geq V$ and stay there for a determined amount of time: until $F(v)$ after the first process enters $v$.
   Formally, $\forall v \geq V.E_{first}(v+1)\geq E_{first}(v)+F(v)$.
      
#### <span style="font-variant:small-caps;">FastSync</span> 

###### Implementation





## Reference
1. [Making Byzantine Consensus Live](https://software.imdea.org/~gotsman/papers/bftlive-disc20.pdf), DISC, 2020


