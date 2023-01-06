---
layout: post
title: "Simplified Bitcoin"
categories: consensus
---
In this post, we are going to see how malicious miners of the Bitcoin network can run a double-spend attack.
We first present a simplified version of Bitcoin protocol.

{% include pseudocode.html id="1" code="
\begin{algorithm}
\caption{Simplified Bitcoin - code for miner $m$}
\begin{algorithmic}
    \STATE{$tree \leftarrow genesisBlock$}

    \PROCEDURE{Main}{}
        \WHILE{$true$}
            \STATE{$transactions \leftarrow \mathtt{getSetOfTransactions()}$}
            \IF{$transactions$ are valid}
                \STATE{create a block for $transactions$}

            \ENDIF

            \STATE{$lc \leftarrow \mathtt{getLongestChain()}$}
            \STATE{$x$}
            \STATE{}
        \ENDWHILE
    \ENDPROCEDURE



    \UPON{receiving a block $b$}
        \IF{$b$ is a valid}
            \STATE{$p$ add to the $tree$}
        \ENDIF
    \ENDUPON
\end{algorithmic}
\end{algorithm}
" %}

Let's see the definition of double-spend attack.



Now we are ready to see how malicious miners can run a double-spend attack.

