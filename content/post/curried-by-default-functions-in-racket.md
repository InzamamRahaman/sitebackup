+++

title = "Curried by Default Functions in Racket"
date = "2015-12-07T12:02:52.721000"
+++

(I plan on writing several more posts in Macros in Racket and Macros in other languages like Julia and Elixir; hence the discussion of theory before the code.)

Sometimes, when I am not working in an ML-dialect or in a heavily influenced Haskell flavoured programming language, I defining functions that are curried by default. In said languages, a function defined to accept multiple arguments can be conceptualised to be a function that accepts a single argument and then return another function that accepts a single argument, and so on and so forth. 

The type signatures of functions in these languages expression this notion quite clearly. For example, in our example below, we can see that add has the type signature  Num a => a -> a -> a, and when we define add1, it has the type signature Num a => a -> a.

<script src="https://gist.github.com/InzamamRahaman/c166ba8c8252f5f52025.js"></script>

Most other functional languages, even those that do not curry functions by default, have some facility to curry functions after their creation or specialised syntax to create curried functions. In Racket and Clojure, the higher-order functions [curry](http://docs.racket-lang.org/reference/procedures.html#%28def._%28%28lib._racket%2Ffunction..rkt%29._curry%29%29) and [partial](https://clojuredocs.org/clojure.core/partial). In Scala, [we have specialised syntax to create a curried function](http://docs.scala-lang.org/tutorials/tour/currying.html). 

When of the coolest properties of LISP-dialects emerges from one of their most (in)famous syntactic quirks - their use of parenthesis. The ubiquity of parenthesis in LISP-dialects is a product of their underlying property - their homioconicity. Not only are lists used extensively in LISP code, LISP code is written *in* LISP lists. This blurs the distinction between code and data, allowing us to manipulate code to dynamically extend the language's syntax within the confines of the syntactic convention of representing code in lists. 

Code that generalises over syntax is usually referred to a macro. Macros can abstract away more primitive syntactic forms in the same way functions can abstract away more primitive operations and values. Using macros, we can define our own syntactic sugar, growing and shaping the language to better represent our domain or suit our tastes.

Racket, a derivative of Scheme, is renown in the PL community for its powerful features to define macros. The simplest way to define a macro in Racket is using  `define-syntax-rule`. When creating a macro using `define-syntax-rule`, we simply supply a description of the syntactic form we are creating and a description of how we want this form to be mapped to a more primitive form. 

<script src="https://gist.github.com/InzamamRahaman/c46690c643e1de23d791.js"></script>

In the above example `(define-curry (name a b ...) body)` gives a description of our new syntactic form. Here, `define-curry` gives the name of our custom syntactic form (to complement the in-built `define`), `(name a b ...)` gives the name of our curried function as well as the argument list, and `body` represents the function body. In `(define name (curry (lambda (a b ...) body)))`, we give the syntactic form that we want to above to map onto, using the pieces we pulled apart to assemble code using more primitive and pre-defined syntactic forms.




