---
layout: post
title: "The Essence of Algorithms"
categories: algorithm
---

**Question)** What is the essence of algorithms?

We can ask this question differently: what is the essence of programming languages, like Java and Python?

**Short Answer)** The essence of algorithms is to provide some mechanisms in order to 
(1) define some initial state(s), 
(2) define some new state(s), possibly based on the defined states, and 
(3) determine some of the defined state(s) as the output. 

<hr>

**Example 1)**
Consider the following simple program written in Python.

{% highlight python %}
# a list is defined and initialized. 
# the sum of the elements of the list is calculated.
# the summation is printed.

lst = [1, 2, 3]   
s = 0
for item in lst:
    s += item
print(s)

# output: 6
{% endhighlight %}

It is straightforward to see that this program creates the following states 
(each state is created by executing each line of the program):

@startuml
[*] -> S1
S1 : lst[0]=1, lst[1]=2, lst[2]=3
S1 -> S2
S2 : lst[0]=1, lst[1]=2, lst[2]=3, s=0
S2 -> S3
S3 : lst[0]=1, lst[1]=2, lst[2]=3, s=0, item=1
S3 --> S4
S4 : lst[0]=1, lst[1]=2, lst[2]=3, s=1, item=1
S4 -left-> S5
S5 : lst[0]=1, lst[1]=2, lst[2]=3, s=1, item=2
S5 -left-> S6
S6 : lst[0]=1, lst[1]=2, lst[2]=3, s=3, item=2
S6 --> S7
S7 : lst[0]=1, lst[1]=2, lst[2]=3, s=3, item=3
S7 -> S8
S8 : lst[0]=1, lst[1]=2, lst[2]=3, s=6, item=3
S8 -> S9
S9 : s=6
S9 -> [*]
@enduml

In this program, S1, S2, and S9 are defined explicitly.
A mechanism provided by Python called `for loop` is used to create the other states automatically.

<hr>

