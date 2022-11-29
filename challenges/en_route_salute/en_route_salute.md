# En Route Salute

### The challenge specification
Commander Lambda loves efficiency and hates anything that wastes time. She's a busy lamb, after all! She generously rewards henchmen who identify sources of inefficiency and come up with ways to remove them. You've spotted one such source, and you think solving it will help you build the reputation you need to get promoted.

Every time the Commander's employees pass each other in the hall, each of them must stop and salute each other - one at a time - before resuming their path. A salute is five seconds long, so each exchange of salutes takes a full ten seconds (Commander Lambda's salute is a bit, er, involved). You think that by removing the salute requirement, you could save several collective hours of employee time per day. But first, you need to show her how bad the problem really is.

Write a program that counts how many salutes are exchanged during a typical walk along a hallway. The hall is represented by a string. For example:
"--->-><-><-->-"

Each hallway string will contain three different types of characters: '>', an employee walking to the right; '<', an employee walking to the left; and '-', an empty space. Every employee walks at the same speed either to right or to the left, according to their direction. Whenever two employees cross, each of them salutes the other. They then continue walking until they reach the end, finally leaving the hallway. In the above example, they salute 10 times.

Write a function answer(s) which takes a string representing employees walking along a hallway and returns the number of times the employees will salute. s will contain at least 1 and at most 100 characters, each one of -, >, or <.

### Test cases

Input:

`">----<"`

Output:

`2`

Input:

`"<<>><"`

Output:

`4`

Input:

`"--->-><-><-->-"`

Output:

`10`

Input:

`"-<-><-->-"`

Output:

`2`

Input:

`"-<->><-->>-<<->->"`

Output:

`20`

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
