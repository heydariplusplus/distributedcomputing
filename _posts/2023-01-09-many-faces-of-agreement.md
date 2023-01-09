---
layout: post
title: "Many Faces of Agreement"
categories: agreement
---
In this post (that evolves through time), we present different variants of the distributed agreement problem.

## 1. Broadcast
Broadcast abstractions are used to reliably send a single message from a designated correct sender to all correct processes.

### 1.1 Probabilistic Broadcast
Assume that a process $$p$$ broadcasts a message $$m$$.
For any $$\epsilon \in [0,1]$$, we say that probabilistic broadcast is $$\epsilon$$-secure if the following properties are satisfied:
1. **No duplication**: No correct process delivers more than one message.
2. **Integrity**: If a correct process delivers a message $$m$$, and $$p$$ is correct, then $$m$$ was previously broadcast by $$p$$.
3. **$$\epsilon$$-Validity**: If $$p$$ is correct, and $$p$$ broadcasts a message $$m$$, then $$p$$ eventually delivers $$m$$ with probability at least $$(1 − \epsilon)$$.
4. **$$\epsilon$$-Totality**: If a correct process delivers a message, then every correct process eventually delivers a message with probability at least $$(1 − \epsilon)$$.

[[Reference](https://core.ac.uk/download/pdf/231819228.pdf)]

### 1.2 Probabilistic Consistent Broadcast
For any $$\epsilon \in [0,1]$$, we say that probabilistic consistent broadcast is $$\epsilon$$-secure if the following properties are satisfied:
1. **No duplication**: No correct process delivers more than one message.
2. **Integrity**: If a correct process delivers a message $$m$$, and $$p$$ is correct, then $$m$$ was previously broadcast by $$p$$.
3. **$$\epsilon$$-Total validity**: If $$p$$ is correct, and $$p$$ broadcasts a message $$m$$, then every correct process eventually delivers $$m$$ with probability at least $$(1 − \epsilon)$$.
4. **$$\epsilon$$-Consistency**: Every correct process that delivers a message delivers the same message with probability at least $$(1 − \epsilon)$$.

[[Reference](https://core.ac.uk/download/pdf/231819228.pdf)]