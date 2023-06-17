# Don't Get Volunteered!

## Challenge Specification

```
As a henchman on Commander Lambda's space station, you;re expected to be resourceful, smart, and a quick
thinker. It's not easy building a doomsday device and ordering the bunnies around at the same time,
after all! Inorder to make sure that everyone is sufficiently quick-witted, Commander Lambda has installed
new flooring outside the henchman dormitories. It looks like a chessboard, and every morning and evening
you have to solve a new movement puzzle in order to cross the floor. That would be fine if you got to
be the rook or the queen, but instead, you have to be the knight. Worse, if you take too much time
solving the puzzle, you get "volunteered" as a test subject for the LAMBCHOP doomsday device!

To help yourself get to and from your bunk every day, write a function called solution(src, dest) which
takes in two parameters: the source square, on which you start, and the destination square, which is
where you need to land to solve the puzzle. The function should return an integer representing the
smallest number of moves it will take for you to travel from the source square to the destination
square using a chess knight's moves (that is, two squares in any diection immediately followed by one
square perpendicular to that direction, or vice versa, in an "L" shape). Both the source and
destination squares will be an integer from 0 and 63, inclusive, and are numbered like the example
chessboard below:

-------------------------
| 0| 1| 2| 3| 4| 5| 6| 7|
-------------------------
| 8| 9|10|11|12|13|14|15|
-------------------------
|16|17|18|19|20|21|22|23|
-------------------------
|24|25|26|27|28|29|20|31|
-------------------------
|32|33|34|35|36|37|38|39|
-------------------------
|40|41|42|43|44|45|46|47|
-------------------------
|48|49|50|51|52|53|54|55|
-------------------------
|56|56|58|59|60|61|62|63|
-------------------------
```

## Sample Testcase

```
Input: Solution.solution(19, 36)
Output: 1

Input: Solution.solution(0, 1)
Output: 3
```

## Understanding

To make the problem easier try converting the numbers into matrix indices. For example,

```
19 -> (19/8), (19%8) -> 2, 3
36 -> (36/8), (36%8) -> 4, 4
```

Note: Here the `8` denotes the number of rows and columns.

First we have to understand the knight's moves. A knight moves 1 unit in a row and 2 units in a column or 2 units in a row and 1 unit in a column. So we can create
a checklist to explore all the possible moves.

```
[[1, 2], [2, 1], [-1, -2], [-2, -1], [1, -2], [-1, 2], [-2, 1], [2, -1]]
```

Now we need to perform a `Breadth First Search` of the chessboard to find the minimum number of moves. <br>
`Tip: Use a Queue data structue to perform BFS`

We also need to check if the generated possible values are valid.
A valid indices for this problem will be

```
For (i, j)
0 <= i < 8
0 <= j < 8
```

## Steps

-   Create a Queue data structure that stores a tuple containing `current_row`, `current_column` & `number_of_steps` and store the initial values.
-   Create a Set that stores a `row, column` pair to check if the current state has already been visited.
-   Perform BFS.
-   When removing a tuple from the queue, if the tuple contains the `dest_row` and `dest_column`, we return the `num_of_steps`.
-   Else, we generate all the posible moves from the current position.
-   We loop through each move and if the move is valid and if the state has not occurred before we add the state into the Queue and mark the state as visited.
-   We will have a guaranteed solution.

## Tips

-   You can use `deque` in _Python_ and use the `popleft()` method to get the front value.
-   In _JAVA_, Since it is not possible to store a tuple, you can store an array into the queue or you can create a Tuple class.

```
class Tuple {
    int row, col, steps;

    Tuple(int row, int col, int steps) {
        this.row = row;
        this.col = col;
        this.steps = steps;
    }
}
```

Now you can initialize the queue like

```
Queue<Tuple> q = new LinkedList<>();
```

I Hope This Helps ;-)
