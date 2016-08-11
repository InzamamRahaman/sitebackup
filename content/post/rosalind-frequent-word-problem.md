+++
title = "Rosalind - Frequent Word Problem"
date = "2015-04-23T10:22:47"
+++


This is a naive solution to the [Frequent Words Problem on Rosalind’s Bioinformatics Textbook track](http://rosalind.info/problems/1a/).

It just boils down:

1. Finding all sub-strings of length k in the string (handled by *our all-substrings-of-len-k* function)
2. Enumerating the frequencies of the sub-strings we found (handled by the incredibly useful *frequencies* function)
3. Finding the highest frequency (handled by the *(<span class="pl-en">apply</span> max (<span class="pl-en">vals</span> freq-table))* in our *most-frequent-k-mers* function)
4. Iterating through all key-value pairs and getting the keys from all pairs whose values are the same as our highest
5. Using *clojure.string/join* to construct a nice and neat string with all of our most frequent k mers

Step 4 involves using the extract-keys-with-values function that uses clojure’s *for* macro. The for marco allows us to iterate through our map as a sequence, and by using the option supplied by the *:when* keyword, filter the sequence to get our desired sequence of keys.

<div class="gist-for-robots"><script src="http://gist.github.com/029e9d26360df7715ef6.js"></script></div>
