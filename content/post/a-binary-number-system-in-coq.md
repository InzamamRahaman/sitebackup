+++
title = "A 'Binary' Number System in Coq"
date = "2015-07-06T21:25:56"
+++


Things have been pretty hectic for me of late, so I haven’t had as much time to blog in the past few weeks. Since I am interested in programming language theory,  a lecturer of mine recommended that I work through the book Software Foundations by Pierce et al. I did some exercises before, but  lost momentum and failed to complete it the last time. There are so many cool concepts in Software Foundations, so this time I really want to try harder to complete it. Maybe blogging about it can help as a forcing function :P.

Towards the end of the first chapter, we are presented with an excercise to implement an inductive type to represent natural numbers using a “binary” system with three cases:

1. 0
2. 2 times a “binary number”
3. 2 times a “binary number” plus 1

<div class="gist-for-robots"><script src="http://gist.github.com/873f30dbde55eaf5b38b.js"></script></div>We are also tasked with writing two functions:

1. One to increment natural numbers represented in this fashion
2. Another to convert from this system of representation to the normal natural number representation

Much like the natural number examples earlier in the book, we write both of these functions by considering each case individually.

Incrementing *Zero* should give us a valid representation for 1, which turns out to be *(TwicePlusOne Zero).* For *Twice n, **TwicePlusOne n *provides our incremented representation. Finally, we can consider *TwicePlusOne n.* This one is pretty simple as well  as \(2n + 1 + 1\) is \(2n + 2\), which is \(2 \left( n + 1 \right)\). This is simply twice the incremented value of \(\), represented as *(Twice (incr_bin n)).*

<div class="gist-for-robots"><script src="http://gist.github.com/456446868a9ea3a09565.js"></script></div>The implementation for the conversion function is incredibly straightforward:

<div class="gist-for-robots"><script src="http://gist.github.com/c671f1215fc56a9347b4.js"></script></div>The unit tests were also pretty straightforward, requiring only the use of the **reflexivity** tactic. Thus far, the exercises have been really easy. I suspect that would change when I begin tackling Chapter 2, where we start dealing with proofs by induction in Coq.

Anywho, bye for now 😀

 

 


