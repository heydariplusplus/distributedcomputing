---
layout: post
title: "Simplified Bitcoin"
categories: consensus
---
In this series of posts, we present a simplified version of Bitcoin (we highlight  the places that are simplified).
Bitcoin is a set of clients $$\mathcal{C}$$ and miners $$\mathcal{M}$$ that execute a distributed protocol called Nakamoto consensus.
Clients can issue transactions, and miners are responsible for processing, validating, and storing the transactions.

> Simplification 1. We assume that the role of clients is played with miners, i.e., there is only one type of entity: miners.

The communication network of the miners forms a [connected graph](https://mathworld.wolfram.com/ConnectedGraph.html#:~:text=A%20connected%20graph%20is%20graph,is%20said%20to%20be%20disconnected.){:target="_blank"}.
The system is permissionless, i.e., miners can join and leave the system at any time.
It follows that $$\mathcal{M}$$ is dynamic.

> Simplification 2. We assume that $$\mathcal{M}$$ is static and all miners know each other in advance.
> Also, the communication network is a complete graph, i.e., every two miners are directly connected.

It should be mentioned that the protocols that we present later in this post work even without Simplification 2.
We present this simplification to avoid explaining how miners can find each other, how new miners can join the system, and so on.

> Simplification 3. We assume that the system is [synchronous](https://decentralizedthoughts.github.io/2019-06-01-2019-5-31-models/){:target="_blank"}, i.e., the communication delay is bounded by a known constant $$\Delta \geq 0$$.

> Simplification 4. Each miner is either honest or [Byzantine](https://en.wikipedia.org/wiki/Byzantine_fault){:target="_blank"} (malicious). Honest miners follow the protocols and don't crash.

Each miner $$m \in \mathcal{M}$$ has some amount of money, denoted by $$m.money \geq 0$$.
Each miner $$s$$ can transfer some of its money to another miner $$d$$.
To do so, $$s$$ must create a transaction that is instantiated from the following data structure.

$$Transaction \{$$ \\
   &emsp; $$source \leftarrow \perp$$           &emsp; \\* the miner that wants to send some of its money \*\\ \\
   &emsp; $$destination \leftarrow \perp$$      &emsp; \\* the miner that will receive the money \*\\ \\
   &emsp; $$money \leftarrow \perp$$            &emsp; \\* the amount of money \*\\ \\
$$\}$$

When a miner creates a transaction, it broadcasts the transaction in the network.
Since we assumed that the system is synchronous, all miners will deliver the transaction up to time $$ t + \Delta$$, where $$t$$ is the time that the miner broadcasted the transaction.
After delivering a transaction $$tx$$, each miner $$m$$ creates a block and writes $$tx$$ in the block.
Then, $$m$$ tries to find a proof of work for that block.
If $$m$$ successes in finding such a proof, then it broadcast the block in the network.
Blocks form a tree.
Each block is an instance of the following data structure.

$$Block \{$$ \\
   &emsp; $$pow \leftarrow \perp$$ \\
   &emsp; $$parent \leftarrow \perp$$ \\
   &emsp; $$children \leftarrow \emptyset$$ \\
   &emsp; $$transactions \leftarrow \emptyset$$ \\
$$\}$$


takes the following steps:
1. $$m$$ creates an instance $$b$$ of data structure $$Block$$
2. $$m$$ creates another transaction $$award$$ such that $$award.source \leftarrow m$$, $$award.desitination \leftarrow m$$, and $$award.money \leftarrow 1$$.
3. $$m$$ assigns $$b.transactions \leftarrow \{tx, award\}$$.
4. $$m$$ assigns $$b.parent \leftarrow getLeafInLongestChain()$$.

After creating such a block, each miner tries to fill the $$pow$$ field of the block.
In the next post, we elaborate on how such a filed can be filled.
It must be assumed that there is special block called genesis block that is the root of the tree constructed by blocks.


<!-- Each process can invoke PoW() operation.
$$PoW-difficulty$$ corresponds to the amount of computation needed to solve a PoW puzzle and is the same for all miners. -->


## Tree construction using instances of *Block*


## Random oracle

## Simplified Bitcoin protocol

{% include pseudocode.html id="1" code="
\begin{algorithm}
\caption{Random Oracle - code for miner $m$}
\begin{algorithmic}
    \STATE{$difficulty \leftarrow 10^{-10}$}

    \PROCEDURE{compute}{$block$}
        \WHILE{$true$}
            \STATE{$rand \leftarrow uniformRandomGenerator(0,1)$}
            \IF{$rnd < difficulty$}
                \STATE{$block.pow \leftarrow rnd$}
                \STATE{return $block$}
            \ENDIF
        \ENDWHILE
    \ENDPROCEDURE

    \PROCEDURE{verify}{$block$}
        \WHILE{$true$}
            \STATE{$rand \leftarrow uniformRandomGenerator(0,1)$}
            \IF{$rnd < difficulty$}
                \STATE{return $\langle block, rnd \rangle$}
            \ENDIF
        \ENDWHILE
    \ENDPROCEDURE

\end{algorithmic}
\end{algorithm}
" %}

{% include pseudocode.html id="2" code="
\begin{algorithm}
\caption{Simplified Bitcoin - code for miner $m$}
\begin{algorithmic}
    \STATE{$tree \leftarrow genesisBlock$}

    \PROCEDURE{Main}{}
        \WHILE{$true$}
            \STATE{$transactions \leftarrow \mathtt{getSetOfTransactions()}$}
            \IF{$transactions$ are valid}
                \STATE{$leaf \leftarrow getLeafInLongestChain(tree)$}
                \STATE{$newBlock \leftarrow createBlock(transactions, leaf)$}
                \STATE{$win \leftarrow false$}
                \STATE{fork $win \leftarrow $ PoW()}
                \WHILE{$true$}
                    \IF{someone else won}
                        \STATE{terminate PoW}
                        \STATE{break}
                    \ENDIF
                    \IF{$win = true$}
                        \STATE{broadcast block}
                    \ENDIF
                \ENDWHILE
            \ENDIF
        \ENDWHILE
    \ENDPROCEDURE

    \UPON{receiving a block $b$}
        \IF{$b$ is a valid}
            \STATE{add $p$ to $tree$}
        \ENDIF
    \ENDUPON
\end{algorithmic}
\end{algorithm}
" %}


