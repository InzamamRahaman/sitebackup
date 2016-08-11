+++
title = "Rosalind - Counting nucleotides"
date = "2015-04-22T23:18:10"
+++


A solution in F#

<div class="gist-for-robots"><script src="http://gist.github.com/12f1930df776009b0c8a.js"></script></div>To make things simple, I modeled the counts of the nucleotides as a 4-tuple of integers, and wrote a function – addToCount- that incremented the appropriate field of the tuple as a fold threads the 4-tuple across the string representing the DNA strand.

A solution in Clojure

<div class="gist-for-robots"><script src="http://gist.github.com/2e5e14daa02b649b256c.js"></script></div>In Clojure, the frequencies function can do the heavy lifting, and we just need to break-out the counts in the needed order


