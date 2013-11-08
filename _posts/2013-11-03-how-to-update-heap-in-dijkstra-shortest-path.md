---
layout: post
title: "How to Update Heap in Dijkstra Shortest Path"
description: ""
category : algo
tags: ["algo.js"]
---
{% include JB/setup %}

> When we use a __heap__ to improve the runing time of Dijkstra shortest path algorithm from $O(nm)$ to $O(n \ln m)$, we may find that it is not easy to keep the heap in heap order just using insert() or delete(). This post describes the update of that heap.
>
>
> I suppose that you might:
> *	know how to wirte Dijkstra algorithm with $O(nm)$ running time, and
> *	know how to use heap.
>
>
> 为了将Dijkstra最短路径算法的时间复杂度从 $O(nm)$ 降低到 $O(n \ln m)$ ，我们可以使用 __heap__ 。不过迭代中的每一次更新heap的过程，我们需要一些技巧来保持heap的有序性。本文就会指出该技巧，并且解释我在算法代码中的一些[变动] [3]。

<!--more-->

To speed up the finding minimum length of path in each stage in Dijkstra shortest path algorithm, we can use a binary heap to store frontier path, according to many words, like [_Heap Application_] [1], or Tim Roughgarden's [algorithm course] [2].

{% gist 6200004 %}

It sounds easy, however the 1st revision of `dijkstra()` in Algo.js is failed to update heap correctly, where I just update the value of one vertex without keeping heap order.

{% gist 6200035 %}

How to update and keep heap order?

While, when we update the MinHeap, it means that we may replace the item at that index with a value __LESS__ than the origin one. According to the definition of minimum binary heap, each parent is less than their children. (see picture from [Wikipedia] [1])

![Min Heap](http://upload.wikimedia.org/wikipedia/commons/6/69/Min-heap.png)

So, if we replace $17$ with a __LESS__ value called $x$. $x$ is still less than its children, but $x$ may be less than $2$ (its parent). As the algorithm of `push()` of heap, we need to exchange $x$ with its parent, great-parent..., until heap is ordered. That is:

__Using `heap.swim()` to update that heap.__ (see [diff][3] of revision)

{% gist 6200086 %}

<div class="post-content lang zh-cn">

简言之，算法的每次迭代，都是用较小的值去更新原来的heap，所以我们应该调用 <code>heap.swim()</code> 来维持heap的有序性。

</div>

[1]: http://en.wikipedia.org/wiki/Heap_(data_structure)#Applications	"Wikipedia"
[2]: https://www.coursera.org/course/algo 								"Algorithms: Design and Analysis, Part 1"
[3]: https://goo.gl/NssHNy                                              "Diff of Algo.js"