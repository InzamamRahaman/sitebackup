+++
title = "Reading FASTA format sequences"
date = "2015-05-01T07:12:41"
+++


Many problems on Rosalind require you to read in genetic strings in [fasta format](http://rosalind.info/glossary/fasta-format/). One way to accomplish this is to read in the entire file as a string and then to split the string appropriately to extract the information and store it an appropriate data structure.

A functional solution in Clojure can be broken down into the following steps:

1. Split the string on the instances of  ‘>’
2. Split each of these “children” strings based on whitespace using *map*
3. Reduce each of these strings appropriately to our data structure that represents the fasta record
4. Solve the harder problem ![:)](http://kyledef.org/inzamam/wp-includes/images/smilies/simple-smile.png)

The above is performed by the following Clojure code:

 

<div class="gist-for-robots"><script src="http://gist.github.com/5f52804a408e6a2ea949.js"></script></div>
