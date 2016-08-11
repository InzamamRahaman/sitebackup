+++
title = "Structs in C"
date = "2015-01-21T15:11:35"
+++


A lot of entities in the universe, both abstract and concrete, cannot be adequately represented by  simple data types such as integers and characters. However, their properties can be represented by such data types. Trying to model these properties as disconnected variables or arrays is far from ideal solution; in addition to being difficult to maintain, such a strategy would not properly express the microcosm we wish to model and would not be conducive to problem solving. Consequently, we may represent such entities by aggregating their representations of their properties into custom data types that we can use as abstractions of those entities. In the C programming language, this is achieved using structs.

To create the blueprint for a struct, we use the keyword struct, giving the struct a name and describing its properties with appropriate types and names as fields within the struct.

<div class="gist-for-robots"><script src="http://gist.github.com/785204a4154af41a3761.js"></script></div>To create an “instance” of the struct, we once again use the struct keyword. Upon creating an instance, the fields wouldn’t contain meaningful data, so we need to access the fields somehow to put meaningful data into them: we typically access the fields of a struct using dot notation.

<div class="gist-for-robots"><script src="http://gist.github.com/aec31c9f3ae6477c4221.js"></script></div>Typing struct every time we want to create an instance of a struct would be tedious, so we can get around it by using typedef.

**Using typedef**

The keyword typedef allows us some means of aliasing a type to help us better model a domain. For instance, suppose that we have a need to compute the number of ounces of a liquid given the number of full cups. We can alias unsigned int as cup and alias unsigned int again as ounce to express the domain better.

<div class="gist-for-robots"><script src="http://gist.github.com/a75dc35afafb1459ee41.js"></script></div>Another common use is using typedef in tandem with struct with creating the blueprint for a particular type of struct. We use typedef right before using the struct keyword, and write the name for our custom type after the fields of the struct have been described. We may also, under most conditions, omit the struct name after the struct keyword.

<div class="gist-for-robots"><script src="http://gist.github.com/c7a5aa82d3744538da69.js"></script></div>**Example #1: Complex numbers**

Complex numbers may be considered as comprising two parts:

1. A real component
2. An imaginary component

This is very easy to map to a struct – we only have two properties that we can represent using double to care about

<div class="gist-for-robots"><script src="http://gist.github.com/2262af23f63e289da5bf.js"></script></div>Initializing the contents of a type of struct is a process that we are going to do quite a bit, so it would make sense to write a function to abstract away that functionality for us.

<div class="gist-for-robots"><script src="http://gist.github.com/51f95b8cbc496f662dad.js"></script></div>Complex numbers support addition, subtraction, multiplication, and division. In addition, complex numbers also support other operations such as:

1. a modulus operation that finds of the square root of the sum of the square of its real and imaginary parts
2. a conjugate operation that negates the sign of the imaginary part
3. a negation operation that negates the sign of both parts

So, lets define some functions to perform some of these operations

<div class="gist-for-robots"><script src="http://gist.github.com/c699dcbf1b82f7330f3d.js"></script></div>Now for some excercies:

1. Define a function to perform subtraction (hint: recall that for complex numbers a – b is the same as a + (-b))
2. Define a function to compute division
3. Complex numbers may written in the form real_part + imaginary_part(i). Write a function that stores the contents of our complex_t struct as a string in this form (hint: use sprintf)

 


