# 1D Totalistic Cellular Automaton
### Description
Cellular Automaton are discrete models of cell structures in a grid that use rules to determine states. The most famous example of this is Conway's Game of Life, which is a 2D CA (Cellular Automaton) with 2 states, 0 and 1. All models start with some initial state, and subsequent generations are calculated by analyzing "neighborhoods". Typically, a neighborhood consists of adjacent cells but can be extended to non-adjacent cells. For the sake of simplicity, this project only uses adjacent cells as neighbors.

A 1D totalistic CA is a grid of cells where each generation is a vector of cells and whose next state value is determined by indexing a rule string using the sum of the cell state value with the state values of the left and right neighbors. Wraparound is used for edge cells. For a 1D model with `k` states, a rule string is a `(3k-2)`-digit `k`-ary number. 

Example:
```
Start state: 001211020
Rule: 1001210
3 generations:
001211020
101220100
001121000
```


### Implementation
The entire program can be summed up by the following lines:
```
r =: (10#4) #: ? 1048575
s =: ? 500 $ 3
g =: monad def 'r {~ +/ > |.&y each 1 0 _1'
res =: g^:(<500) s 
NB. Final grid will be 500x500
NB. https://code.jsoftware.com/wiki/Vocabulary/hatco#Boxed_Numeric_n_--_decoding_variable-length_records
```

`r` is the rule, `s` is the initial cell array, and `g` is the function for calculating the next generation given a cell array. 
The process within `g` is as follows:
- Compute rotations by `1`, `0`, and `_1` and unbox

```
   > |.&(i.5) each 1 0 _1
1 2 3 4 0
0 1 2 3 4
4 0 1 2 3
```

- Sum reduce with `+/`. This takes the sum of columns, which in turn is the sum of the cell with its neighbors
- Index the rule string `r` using `{~`, note the use of flip `~` here.

This will give us the next generation for any cell array and rule, no matter the states or size. This implementation uses 4 states. In the definition for `r`, the snippet `(10#4)` represents the `(3k-2)`-digit `k`-ary number, where 4 represents base-4 and 10 is the result of `3*4-2`. I chose to deal integers and convert to base-4 instead of something like `? 10 $ 3` simply because I seemed to get a wider breadth of rules when I was playing around.

### Results
You can find the implementation [here](https://gist.github.com/4hg/2b3497365d194f69a02297ce6470bd1f), which includes visualization code, if you wish to play around with different rules. The rules below showcase some unique variety. If you do not wish to use the J code, [this website](https://e4494s.neocities.org/totalistic.html) is good to see the output.

- Rule: 301979, single 1-cell
- Rule: 555062, single 1-cell
- Rule: 404368, random
- Rule: 397559, either start

Overall, this is not a very challenging or complex program, but it is loads of fun. Plus, it is very elegant to solve using J. The world of cellular automaton is vast and has a ton of capabilities. [WireWorld](https://jbaker.graphics/js+canvas/wireworld.html) is a good example of this. However, even 1D CA's can be much more exciting than what I have explored, so this is the perfect to chance to start exploring.
