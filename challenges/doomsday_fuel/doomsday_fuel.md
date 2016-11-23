# doomsday_fuel
### The challenge specification
Making fuel for the LAMBCHOP's reactor core is a tricky process because of the exotic matter involved. It starts as raw ore, then during processing, begins randomly changing between forms, eventually reaching a stable form. There may be multiple stable forms that a sample could ultimately reach, not all of which are useful as fuel.

Commander Lambda has tasked you to help the scientists increase fuel creation efficiency by predicting the end state of a given ore sample. You have carefully studied the different structures that the ore can take and which transitions it undergoes. It appears that, while random, the probability of each structure transforming is fixed. That is, each time the ore is in 1 state, it has the same probabilities of entering the next state (which might be the same state).  You have recorded the observed transitions in a matrix. The others in the lab have hypothesized more exotic forms that the ore can become, but you haven't seen all of them.

Write a function answer(m) that takes an array of array of nonnegative ints representing how many times that state has gone to the next state and return an array of ints for each terminal state giving the exact probabilities of each terminal state, represented as the numerator for each state, then the denominator for all of them at the end and in simplest form. The matrix is at most 10 by 10. It is guaranteed that no matter which state the ore is in, there is a path from that state to a terminal state. That is, the processing will always eventually end in a stable state. The ore starts in state 0. The denominator will fit within a signed 32-bit integer during the calculation, as long as the fraction is simplified regularly.

For example, consider the matrix m:
```
[
  [0,1,0,0,0,1],  # s0, the initial state, goes to s1 and s5 with equal probability
  [4,0,0,3,2,0],  # s1 can become s0, s3, or s4, but with different probabilities
  [0,0,0,0,0,0],  # s2 is terminal, and unreachable (never observed in practice)
  [0,0,0,0,0,0],  # s3 is terminal
  [0,0,0,0,0,0],  # s4 is terminal
  [0,0,0,0,0,0],  # s5 is terminal
]
```
So, we can consider different paths to terminal states, such as:
```
s0 -> s1 -> s3
s0 -> s1 -> s0 -> s1 -> s0 -> s1 -> s4
s0 -> s1 -> s0 -> s5
```
Tracing the probabilities of each, we find that:
```
s2 has probability 0
s3 has probability 3/14
s4 has probability 1/7
s5 has probability 9/14
```
So, putting that together, and making a common denominator, gives an answer in the form of
`[s2.numerator, s3.numerator, s4.numerator, s5.numerator, denominator]` which is:
`[0, 3, 2, 9, 14]`.

#### Test cases

Inputs:
`
(int) m = [[0, 2, 1, 0, 0], [0, 0, 0, 3, 4], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0], [0, 0, 0, 0, 0]]
`

Output:
`
(int list) [7, 6, 8, 21]
`

Inputs:
`
(int) m = [[0, 1, 0, 0, 0, 1], [4, 0, 0, 3, 2, 0], [0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0]]
`

Output:
`
(int list) [0, 3, 2, 9, 14]
`

### Understanding

This challenge may take a couple of read throughs to understand what is actually going on and what is asked of us. Simply put the aim of your program is to output the chances of ending up in a terminal state from starting at state `s0`. Adding up the numbers in a given `int[]` provides us with a denominator which we can then start to create the probability of which state the 'ore' will change into next.

This is what is happening in a visual diagram using the example used in the specification:

![Diagram](./assets/doomsday_dia.png "")

From this diagram we can see that;

* `s0` has a 50% chance to go to `s5` which is a terminal state, and a 50% chance to go to `s1` which is a non-terminal state.
* `s1` has a 44.44% chance to return back to `s0`, a 22.22% chance to go to `s4` which is a terminal state, and then a 33.33% chance to go to `s3` which is another terminal state.
* `s2`, `s3`, `s4` and `s5` are terminal states, states which cannot travel to any other node. You can imagine that they have a 100% chance of remaining in the same state after an evolution.

So starting from `s0` what are the probabilities that we will end up the terminal states? It is a bit tricky to say because of the loop with 's0' and 's1' meaning that the 50% chance `s0` -> `s1` will be effected by this and so will the probabilities of `s1` -> terminal states.

What would be ideal would be just have to chances of reaching terminal nodes and removing all loops.

Well there is a way, and the way I used was a [Markov Absorbing Chain method which is a Markov chain in which _every_ state will eventually reach an absorbing state.](https://en.wikipedia.org/wiki/Absorbing_Markov_chain)

### A Solution

I will not go into detail about how Markov chains work as there are plenty of online sources out there that would do a lot better than what I could do. But let us go through applying the algorithm to the example above which will give you a good insight on how you could maybe apply the same algorithm to your Java or Python solution.
