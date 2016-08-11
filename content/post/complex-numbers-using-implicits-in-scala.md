+++
title = "Complex numbers using implicits in Scala"
date = "2015-09-23T16:33:29.676000"
+++


IMHO, strong static type systems are great! They can help with reasoning about how your system should work, help in modelling your domain, and help in preventing trivial but common and annoying bugs. That being said, having to explicitly cast values can be a bit of bore and make you miss the blithely nature of dynamic typing. One of my favourite features of Scala is the ability to define *implicit* conversions for your custom defined types. Scala also allows you to write *implicit classes* that enable you to extend the behaviour of existing classes! To demonstrate both of these features, we are going to implement a class for complex numbers in Scala, and use an implicit class to extend the behaviour of Double values for us to create our own “pseudo-literal” for complex numbers in Scala.

 

Firstly, let us define our class for complex numbers and a companion object to handle its administration.

<div class="gist-for-robots"><script src="http://gist.github.com/dc9d0a6f122367c8a2d9.js"></script></div>We can then use it as shown below

<div class="gist-for-robots"><script src="http://gist.github.com/810d1e11049dbe841c5c.js"></script></div> 

So far so good :).

Now, with that out of the way, what we came here for.

If we represent any complex number as $a + bi$,
where $a$ is the real part and $b$ is the imaginary part,
then any real number, $r$ can be represented as $r + 0i$. This is our first stop on using implicits. Instead of manually creating a **Complex** from a **Double**, we can define an implicit conversion to handle the grunt work for us and perform the instantiation behind the scenes. In the code below, an implicit conversion is denoted a method, doubleWrapper, is written to handle the instantiation of the Double value’s corresponding Complex value.

<div class="gist-for-robots"><script src="http://gist.github.com/b672a1243597e49cec4b.js"></script></div>Now usage:

<div class="gist-for-robots"><script src="http://gist.github.com/a4cd754b54d658db4403.js"></script></div>If you used the [“official” Scala IDE written by Typesafe and EFPL students](http://scala-ide.org/), you would see the *2* on line 5 would  be underlined to indicate that an implicit conversion will take place.

Now that we have covered the case of representing a real number as a complex number, what about representing an imaginary number as a complex number ? Since we are already using an implicit conversion from **Double** to **Complex**, we would need to use some other means of instantiating a **Complex** in this case. One means of doing so would be to leverage *implicit classes*. As aforementioned, implicit classes allow us to extend the behaviour of an already existing class, decorating the class as needed. [As outlined in the documentation,](http://docs.scala-lang.org/overviews/core/implicit-classes.html) implicit classes must be defined in an *object*, *class*, or *trait*.

<div class="gist-for-robots"><script src="http://gist.github.com/3ccf79b89556305b34bf.js"></script></div>An implicit class constructor takes only a single value, an instance of the class being extended. We then define all additional methods to extend to initial class’ behaviour in the implicit class’ body.

We may then use it like this:

<div class="gist-for-robots"><script src="http://gist.github.com/89f1bcfb9383f4f3e7a1.js"></script></div>.Something important to note would be the import statement on the first line. Since we created the implicit class in the **Complex** object, we need to explicitly import Complex’s contents to access the implicit class **DoubleToComplex**.

Using our implicit conversion and our implicit class in conjunction, we have a nice little trick we can exploit. Recall that a complex number is essentially the sum of a real number and an imaginary number, that $$ a + bi = (a + 0i) + (0 + bi) $$. So, if we use our implicit conversion to get a Complex from a Double, and our implicit class to derive an imaginary number represented as a Complex from a Double using our method **i, **we can use a “pseudo-literal” as shown on line 19 below:

 

<div class="gist-for-robots"><script src="http://gist.github.com/e850373d98cc46a72b2e.js"></script></div> 

Pretty cool, right :D.

The final code for this example can be found [here.](https://github.com/InzamamRahaman/ScalaComplexNumbers)


