# bomb_baby
### The challenge specification
There are two types of bombs: Mach bombs (M) and Facula bombs (F)

The bombs self-replicate via one of 2 ways: For every Mach bomb, a Facula bomb is created; or for every Facula bomb a Mach bomb is created.

i.e. 3 Mach bombs and 2 Facula bombs either produce 3 Mach bombs and 5 Facula bombs, or 5 Mach bombs and 2 Facula bombs. The replication process can be changed each cycle.

You start with 1 Mach and 1 Facula bomb.

Write a function answer(M, F) where M and F are the number of Mach and Facula bombs needed. Return the fewest number of generations (as a string) that need to pass until you reach the required target, or the string "impossible" if this can't be done! M and F will be string representations of positive integers no larger than 10^50.


#### Test cases
Inputs:
`(string) M = "2" (string) F = "1"`

Output:
`(string) "1"`


Inputs:
`(string) M = "4" (string) F = "7"`

Output:
`(string) "4"`

Inputs:
`(string) M = "2" (string) F = "4"`

Output:
`(string) "impossible"`

### Understanding

I guess the best way to describe the problem is to imagine yourself starting with two numbers `F` and `M`, both of which have the value of `1`. At the start you are currently in `step 0`, to progress a step you must either;

* Add the value of `F` onto `M` to calculate a new `M`, _or_,
* Add the value of `M` onto `F` to calculate a new `F`.

Because of the rules of the specification we can create some facts:

* Because of the steps, `F` can never equal `M` if `step n` > `0`.
* If `F` > `M` we know that the last step to take place involved adding `M` to `F`.
* If `M` > `F` we know that the last step to take place involved adding `F` to `M`.

### A Solution

One sure way to solve this challenge is to reverse engineer the two numbers they provide. From the rules to get from `step n` to `step n-1` we just do `F` = `F` - `M` or `M` = `M` - `F` depending if `F` or `M` is larger than the other.

For example:
```
F = 7   M = 4
    F > M ∴ F = 7 - 4
F = 3   M = 4
    M > F ∴ M = 4 - 3
F = 3   M = 1
    F > M ∴ F = 3 - 1
F = 2   M = 1
    F > M ∴ F = 2 - 1
F = 1   M = 1
```

Well.. that is fairly easy, we have a solution already - we know that if we do not end up with `1` and `1` there is no solution.

But... It is pretty slow. The specification mentions that the inputs can be up to 10^50 in size. Now imagine if we had the inputs `F = 1` and `M = 10^50` we would have to do our calculation 10^50 - 1 times, and who knows how long you will be waiting for that to compute.

This problem occurs when `F | M` is larger by several multiples than its counterpart. So how about we see how many times `F | M` fits into `M | F`, then we increase the counter for how many times it be divided into the other one.

For example:
```
F = 31   M = 4
    F > M ∴ F = 31 - 4 * (31 / 4) //Rounded down of course
F = 3    M = 4
...
```

This solution avoids the problem with massive differences in the numbers and greatly optimizes the solution.

I think this challenge is pretty straight forward but the real challenge is making it optimized, I think there is enough in this file for you to go and take a good shot at this problem.

### Summary

In summary the steps for this solution are:

* Find out if the current `step` you are on is _solvable_.

* If it is, find out if `F` or `M` is bigger.

* Divide the smaller one into the bigger one, round down, to find out the multiplier to increase the counter and to subtract the larger one to find the answer faster.

* Repeat steps until you have `1` and `1`, frequently check if _solvable_ using a custom function and finally print out the counter of how many steps it took.

Some tips:

* Write a custom method to check if you can actually solve the problem, i.e. check if > 0, check `F` != `M` ect.

* Expect numbers larger than 10^32! So use `BigInteger` for example.

* If you are testing your solution and the tests are not running and instead you get a generic `error` it is most likely because the solution you have provided is unoptimized and is too slow. Test out your solution with big numbers.

* If you want some good numbers to test edge case, provide a number N and N + 1 as long as they are greater than 1.
Good luck!
