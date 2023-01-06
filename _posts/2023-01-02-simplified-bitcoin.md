---
layout: post
title: "Simplified Bitcoin I"
categories: consensus
---
In this series of posts, we present a simplified version of Bitcoin.
Bitcoin is a set of clients $$\mathcal{C}$$ and miners $$\mathcal{M}$$ that execute a distributed protocol called Nakamoto consensus.
Each client or miner $$p$$ has some amount of money, denoted by $$p.money \geq 0$$.
Clients or miners can issue transactions, and miners are responsible for processing, validating, and storing the transactions.
A transaction is a data structure that indicates how much money is transfered from a $$source$$ client or miner to a $$destination$$ client or miner.

$$Transaction \{$$ \\
   &emsp; $$source \leftarrow \perp$$          &emsp; &emsp; &emsp; \\* the client or miner that wants to send some of its money \*\\ \\
   &emsp; $$destination \leftarrow \perp$$     &emsp; \\* the client or miner that will receive the money \*\\ \\
   &emsp; $$money \leftarrow \perp$$           &emsp; &emsp; &emsp; \\* the amount of money \*\\ \\
$$\}$$

The communication network of $$\{ \mathcal{C} \cup \mathcal{M} \}$$ forms a [connected graph](https://mathworld.wolfram.com/ConnectedGraph.html#:~:text=A%20connected%20graph%20is%20graph,is%20said%20to%20be%20disconnected.){:target="_blank"}.
The system is [permissionless]({{ site.baseurl }}{% link _posts/2022-12-11-permissionless-consensus-without-knowing-n.md %}), i.e., miners can join and leave the system at any time.
It follows that $$\mathcal{M}$$ is dynamic.

> Simplification 1. We assume that $$\mathcal{M}$$ is static and all miners know each other in advance.
> Also, the communication network is a complete graph, i.e., every two members of $$\{ \mathcal{C} \cup \mathcal{M} \}$$ are directly connected.

It should be mentioned that the protocols that we present later in this post work even without Simplification 1.
We present this simplification to avoid explaining how miners can find each other, how new miners can join the system, and so on.

> Simplification 2. We assume that the system is [$$\Delta$$-synchronous](https://dl.acm.org/doi/10.1145/3519270.3538430){:target="_blank"}, i.e., the communication delay is bounded by a known constant $$\Delta \geq 0$$.

> Simplification 3. Each miner is either honest or [Byzantine](https://en.wikipedia.org/wiki/Byzantine_fault){:target="_blank"} (malicious). Honest miners follow the protocols and don't crash.
> Besides, each client is honest.

When a client or miner wants to transfer some money to another client or miner, it creates a transaction.
Then, it broadcasts the transaction in the network.
Since we assumed that the system is $$\Delta$$-synchronous, all miners will deliver the transaction up to time $$ t + \Delta$$, where $$t$$ is the time that the transaction was broadcasted.
After delivering a transaction $$tx$$, each miner $$m$$ take the following steps:
1. Checking the validity of the transaction. If it is valid, go to step 2; otherwise, ignore this transaction.
2. Creating a block for the transaction.
3. Reaching an agreement on this block with other miners.

We first explain how a block can be created for a transaction.
Then, we elaborate on Step 1 and Step 3.
Each block contains the following information:

$$Block \{$$ \\
   &emsp; $$pow \leftarrow \perp$$ \\
   &emsp; $$parent \leftarrow \perp$$ \\
   <!-- &emsp; $$children \leftarrow \emptyset$$ \\ -->
   &emsp; $$transactions \leftarrow \emptyset$$ \\
$$\}$$

Each miner $$m$$ creates an instance of $$Block$$ for a set of valid transactions.

> Simplification 4. We assume that each block contains one transaction.

Assume that miner $$m$$ delivers a transaction $$tx$$.
After creating a block $$b$$, $$b.transactions \leftarrow \{tx\}$$.
To determine the other fields of $$b$$, we must know that:
- There is a special block called the *genesis* block that does not have a parent.
- Miners try to organize blocks in a tree starting from the genesis block.
- The parent of a newly created block is the block that has the longest distance from the genesis block. If there are several of them, one of them is selected randomly.

Let's see an example.
Assume that the current state of the system is as follows.
![init](/assets/img/0001-current_state.png){: style="display:block; margin-left:auto; margin-right:auto; width:250px" }

Since $$block_4$$ has the longest distance from the genesis block, $$b.parent \leftarrow block_4$$.
In the next post, we discuss what is $$pow$$ and how it can be filled.

<!-- and fill $$b$$ as follows:
- Creating another transaction $$award$$ such that $$award.source \leftarrow m$$, $$award.destination \leftarrow m$$, and $$award.money \leftarrow 1$$.
- Assigning $$b.transactions \leftarrow \{tx, award\}$$.
- Assigning $$b.parent \leftarrow getLeafInLongestChain()$$. -->

<!-- Each miner to prove that




Then, $$m$$ tries to find a proof of work for that block.
If $$m$$ successes in finding such a proof, then it broadcast the block in the network.
Blocks form a tree.
Each block is an instance of the following data structure.


After creating such a block, each miner tries to fill the $$pow$$ field of the block.
In the next post, we elaborate on how such a filed can be filled. -->



