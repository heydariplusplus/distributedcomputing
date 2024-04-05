---
layout: post
title: Adopt-Commit and OTR
# subtitle: Excerpt from Soulshaping by Jeff Brown
# cover-img: /assets/img/path.jpg
# thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
tags: [consensus]
# author: Hasan Heydari
---

In this post, 

Many consensus protocols are structured by rounds in which each process typically either adjusts its preference or decides a value in a round.
This kind of protocols can be abstracted as **adopt-commit** objects.
An adopt-commit object supports a single operation, AdoptCommit.
When this operation is invoked by a process with two parameters $r$ and $x$, where $x$ denotes the preference of the process in round $r$, it returns an output of the form (commit, $y$) or (adopt, $y$), where the first value serves as a decision indicating whether the process should commit on the value $y$ or adopt it as its preferred value in subsequent rounds of the protocol.
An adopt-commit object requires to satisfy the following properties:
- Convergence. If all input values are $v$, all outputs are (commit, $v$).
- Coherence. If the output of some operation is (commit, $v$) in round $r$, then every output is either (adopt, $v$) or (commit, $v$).

{% highlight pseudocode linenos %}
preference = input
r = 1
loop
    (b, preference) = AdoptCommit(r, preference)
    if b == commit 
        return preference
    else
        do something to generate a new preference
{% endhighlight %}

#### The OneThirdRule Algorithm
{% highlight pseudocode linenos %}
preference = input
r = 1
loop
    broadcast <r, preference>
    wait until receiving <r, *> from more than 2n/3 processes
    preference = the smallest most often received preference in round r
    if more than 2n/3 of received preferences in round r are equal to x' 
        decide x'
    r = r + 1
{% endhighlight %}

#### The OneThirdRule Algorithm as an Adopt-Commit Object

{% highlight pseudocode linenos %}
preference = input
r = 1
loop
    (b, x) = AdoptCommit(r, preference)
    if b == commit 
        return x
    else
        preference = x
    r = r + 1

function AdoptCommit(r, preference)
    broadcast <r, preference>
    wait until receiving <r, *> from more than 2n/3 processes
    if more than 2n/3 of received preferences in round r are equal to x' 
        return (commit, x')
    else return (adopt, the smallest most often received preference in round r)
{% endhighlight %}