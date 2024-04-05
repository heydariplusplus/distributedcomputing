---
layout: post
title: PaLa vs Doubly-Pipelined PaLa
# subtitle: Excerpt from Soulshaping by Jeff Brown
# cover-img: /assets/img/path.jpg
# thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
tags: [consensus, blockchain]
author: Hasan Heydari
---

|PaLa | Doubly-Pipelined PaLa|
|-----|----------------------|
|based on the rotating leader/proposer approach, in which the leader/proposer is changed in each round | based on the stable leader/proposer approach, in which the leader/proposer is not changed as long as it works well|
|a correct node must wait to collect notarizations for the entire ancestor chain before voting on the next block|nodes are allowed to opportunistically vote for blocks even if not all notarizations have been collected for ancestor blocks, as long as the number of unnotarized blocks at the end is bounded by an opportunistic parameter k, where k = 1 is the basic scheme in Section 3. For finalization, nodes must now wait for roughly k consecutively notarized blocks|

