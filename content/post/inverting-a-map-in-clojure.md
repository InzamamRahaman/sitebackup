+++
title = "Inverting a map in Clojure"
date = "2015-09-21T22:13:59.341000"
+++


While working on a solution to another Rosalind problem, I encountered a situation where I needed to invert the key-value relationships in a map $m$ such that $\forall  k_1, k_2, ..., k_n, v $ such that $ (k_1, v), (k_2, v), ..., (k_n, v) \in m $ we would have $(v, [k_1, k_2, ..., k_n]) \in m' $ where $ m' $ is the inverted map.  I couldn’t find something in Clojure that did exactly what I wanted, so I wrote a simple function to do it for me.

<div class="gist-for-robots"><script src="http://gist.github.com/da702d41a112ec51d723.js"></script></div>At the heart of this solution lies a nifty function in the standard library called [merge-with](https://clojuredocs.org/clojure.core/merge-with) that merges a variable number of maps  while allowing the caller to specify how to handle collisions.

First we can divide our larger map comprising of key-value pairs, $(k_i, v_i)$ into smaller maps $ m_1, m_2, ..., m_i $ comprising of $(v_i, [k_i])$. We then use *merge-with*, specifying *concat* as our function to resolve collisions.


