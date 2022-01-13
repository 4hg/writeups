# Concepts
## A brief skim of J basics

### What to Expect
This shall act as a lightweight starting point to aid you in understand the rest of the writing on this site. This should be read if you are unfamiliar with J because although there will be examples in subsequent writing, I will neglect reiterating explanations of basic concepts and freely use native terminology. 

### Terminology
- **noun** - a non-function data value
- **verb** - an ordinary function such as `+` and `*` that works with data
- **adverb** - an operator that accepts one argument that modifies verbs such as `/`
- **monadic** - used to describe unary functions
- **dyadic** - used to describe infix binary operators
- **rank** - the dimension of a noun, `0` for scalars, `1,2,...` for arrays
- **tacit** - "arguments implied"; point-free

### How J Works
J's operator precedence is unlike mainstream languages. We are used to rules like PEMDAS where multiplication comes before addition and left to right is used for operators of the same precedence, and this is perfectly fine. However, there are *a lot* of functions in J, and having some fleshed out hierarchy of precedence would be insane. Therefore, we simply evaluate all J expressions right to left, with the exception of the use of parentheses which act as usual. 

As described before, the concept of monadic and dyadic operators is another basic foundation of J. Monads will *always* accept a right argument, such as `- 1 -> _1`, and dyads will *always* accept a left and right argument, such as `2 * 4 -> 8`. Operators can also be applied to lists, and if you are familiar with NumPy, then this is will be a comfortable concept. Say we want to apply some function to every item in an array, this might look like `map(f, arr)` in Python or `arr.map{...}` in Ruby, but this sort of explicit mapping is done automatically in J. We can also use operators on two arrays. To showcase, here are some examples:
```
  1 + 1 2 3
2 3 4

  *: 2 3 4
4 9 16

  1 2 3 - 4 5 6
_3 _3 _3 
```


Lastly, J allows very easily to write tacit code, and it is often implicit. This means that we will be making strong use of function combination patterns, and although it is idiomatic, it is not necessary 100% of the time. I will choose to explain things like forks and hooks in other writing as to not bog this page down, but as a brief showcase consider the following example.
```
  - % *: 2
_0.25

  (- % *:) 2
_0.5
```
What you are seeing is different function applications in different contexts. In the first, we have a simple right to left application, so `*:` squares the argument, `%` takes the reciprocal, and `-` negates the value. In the second, we have a tacit form called a monadic fork. The scheme for this is `(f y) g (h y)`, so we negate and square the argument first and pass those values to dyadic `%`, which is division.
