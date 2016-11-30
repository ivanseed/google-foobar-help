# Patrol Salutes
_I have lost the specification for this challenge (and the right name) but I have tried my best to recall everything._
### The challenge specification
In the LAMBCHOP Organization there is a rule that all guards who walk past each other must stop and salute, before continuing in their direction. Commander Lambda would like you to provide a solution that will count the number of exchanged salutes between the guards. Guards move left and right, and when two guards pass an exchange of salutes will happen, and the continue.

Write a program that counts how many salutes are exchanged during a typical walk along a hallway. The hall is represented by a string.

`>` - Represents a guard traveling `left to right`.

`<` - Represents a guard traveling `right to left`.

`-` - Represents no guard on this tile.

For example:

`"--->-><-><-->-"`

Here we have 4 guards traveling right and two guards traveling left and in total there will be `5` exchanged salutes.

### Test cases

Input:

`"--->-><-><-->-"`

Output:

`5`

Input:

`"-<-><-->-"`

Output:

`1`

Input:

`"-<->><-->>-<<->->"`

Output:

`10`

### Understanding
This is quite a simple challenge and can be done in several ways. Basically choose whether to start on the _left_ of the string or the _right_ of the string.

If starting on the _left_ for every `>` you come across look to all the characters to the __right__ of it, and every `<` you find to the _right_ increment `salutesExchanged` (which would be a simple counter). Repeat this process until you react the end of the string.

Starting on the _right_ is just the opposite, for every `<` you come across look to all characters to the __left__ and for every `>` increment the counter. Repeat till you come to the end.

### A Solution
A simple, elegant solution can avoid having to keep checking what is on the _left_ or the _right_ of a guard multiple times.

For this let us start on the __left__. Our first step is to first find and count all guards traveling `<`. For example:

`"<-><->-<<"`

Would get us `4` guards going left, and this is the first time we have to go through the entire string. Now that we have the counter for guards going left, lets call it `multiplier`, we can now easily find the total exchanges with one more pass through of the string.

Again let us start from the __left__, we have `multiplier` that equals 4, and for every `-` we encounter - do nothing. For every `>` we encounter add `multiplier` to the counter which you are going to return; `exchanges`. For every `<` you come across you will want to decrement the `multiplier` as any `>` you find after it will never come across the one you just subtracted.

Here is the steps:

`multiplier = 4`
`exchanges = 0`

* char 0 : (`<`) - Decrement `multiplier`, so it now equals 3.

* char 1 : (`-`) - Do nothing.

* char 2 : (`>`) - `exchanges += multiplier`. `exchanges = 3`

* char 3 : (`<`) - Decrement `multiplier`, so it now equals 2.

* char 4 : (`-`) - Do nothing.

* char 5 : (`>`) - `exchanges += multiplier`. `exchanges = 5`

* char 6 : (`-`) - Do nothing.

* char 7 : (`<`) - Decrement `multiplier`, so it now equals 1.

* char 7 : (`<`) - Decrement `multiplier`, so it now equals 0.

The answer we get is `5` which is correct, you can do it yourself manually and you get the same answer.

### Summary

In summary it is a fairly straight forward challenge, steps include:

* Calculate how many are going in one direction.

* Then start from the direction they are going to, for every `-` you come across do nothing, for a guard you come across going in the opposite direction add your multiplier to the counter you will return.

* For each guard you find going in the direction you calculated the multiplier for, decrement the multiplier.

* Stop when multiplier = 0.

Some tips:

* Char At (n) is O(1).

* Stop when your multiplier is 0, don't bother checking the rest of the string.

Bit hard to explain but I hope the solution got its way across, again sorry for losing the specification, if you have it feel free to edit this .md and include it.
