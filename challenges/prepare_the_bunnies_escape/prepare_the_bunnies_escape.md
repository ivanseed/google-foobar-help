# prepare_the_bunnies_escape
### The challenge specification
You're awfully close to destroying the LAMBCHOP doomsday device and freeing Commander Lambda's bunny prisoners, but once they're free of the prison blocks, the bunnies are going to need to escape Lambda's space station via the escape pods as quickly as possible. Unfortunately, the halls of the space station are a maze of corridors and dead ends that will be a deathtrap for the escaping bunnies. Fortunately, Commander Lambda has put you in charge of a remodeling project that will give you the opportunity to make things a little easier for the bunnies. Unfortunately(again), you can't just remove all obstacles between the bunnies and the escape pods - at most you can remove one wall per escape pod path, both to maintain structural integrity of the station and to avoid arousing Commander Lambda's suspicions.

You have maps of parts of the space station, each starting at a prison exit and ending at the door to an escape pod. The map is represented as a matrix of 0sand 1s, where 0s are passable space and 1s are impassable walls. The door out of the prison is at the top left (0,0) and the door into an escape pod is at the bottom right (w-1,h-1).

Write a function answer(map) that generates the length of the shortest path from the prison door to the escape pod, where you are allowed to remove one wall as part of your remodeling plans. The path length is the total number of nodes you pass through, counting both the entrance and exit nodes. The starting and ending positions are always passable (0). The map will always be solvable, though you may or may not need to remove a wall. The height and width of the map can be from 2 to 20. Moves can only be made in cardinal directions; no diagonal moves are allowed.

#### Test cases

Inputs:
`
(int) maze = [
[0, 1, 1, 0],
[0, 0, 0,1],
[1, 1, 0, 0],
[1, 1, 1, 0]
]
`

Output:
`
7
`

Inputs:
`
(int) maze = [[0, 0, 0, 0, 0, 0], [1,1, 1, 1, 1, 0], [0, 0, 0, 0, 0, 0], [0, 1, 1, 1, 1, 1], [0, 1, 1, 1, 1, 1], [0,0, 0, 0, 0, 0]]
`

Output:
`
11
`

### Understanding

The challenge doesn't seem so hard it is a simple maze navigation problem to begin with which a simple Dijkstra algorithm could solve by finding the shortest path from the start of the maze to the exit. Which is great! There are several examples online of optimized Dijkstra algorithm implementations.

_I am not going to explain Dijkstra so I recommend the following:_
* _[Article by Lars Vogel: Dijkstra's shortest path algorithm in Java - Tutorial](http://www.vogella.com/tutorials/JavaAlgorithmsDijkstra/article.html)_
* _[Short and sweet video by Nathaniel Fan: Dijkstra's Algorithm](https://www.youtube.com/watch?v=gdmfOwyQlcI)_

However there is one problem we must overcome: the removable wall.

The fact that one wall can be removed is a real curve ball, but there is still a fairly straight forward way to solve this - however this following solution may not be the most optimized way of doing so. However after you can come to a solution with this you could build upon this foundation to get a more optimized answer.

### A Solution

First of all let us convert our `int[][]` to nodes that are connected and have __weights__. For example:
`
(int) maze = [
[0, 1, 1, 0],
[0, 0, 0,1],
[1, 1, 1, 0],
[1, 1, 1, 0]
]
`

Would look like:

![Graph](./assets/graph.png "Graph after compute")

_Ignore the one way arrows, you can travel both ways._

Green nodes represent normal tiles, red tiles represent walls which are not passable. But in our implementation let us include them but with a very high travel cost so that the algorithm will always favor traveling to green nodes. __(Since we can only get 10x10 a weight of 1000 is more than enough)__

First of all let us calculate all the weights from node `A` to all other nodes, even the nodes that represent walls and this will get us the shortest distance it takes us to get to every other node.

__Once you find the weight to `Q` (the exit) you can stop calculating nodes that have weight that is equal or greater as there is no way they will have a solution to the shortest path, you need to avoid the cost of walls though somehow.__

So from our diagram let us write the weights in the nodes from `A` to all the nodes:

![Graph](./assets/graphweight.png "Graph after weight")

So we found the shortest path from `A` to `Q` but we know that we could remove a wall, and removing a wall could potentially get us a shorter path. So let us calculate paths from __all walls to the end to see if they have a shorter distance to the end__.

But let us _keep_ the weight we already have calculated __in the first weight calculation__ and subtract `999` to counter the wall traversal cost to that node. Then if that weight is less than our current lowest path (which is `1005`) then replace that as the new lowest path. Only bother doing this calculation if the `start weight < current lowest`.

For example calculating one wall (starting from yellow and `-999`):

![Graph](./assets/wallcalc1.png "Graph after wall start")

Here we did not improve the shortest path. But if we keep doing this on all the walls maybe eventually we will find a shorter path: ...

![Graph](./assets/wallcalc2.png "Graph after wall start")

_Note how I still use the original weight `-999` for the wall rather than a potentially calculated weight from another wall._

This solution will always work as you pretty much exhaust every possibility by iterating through all the walls and calculating the path it takes to get from the `wall node` to the `exit node`. Sometimes you never need to remove a wall but this approach will force you to still do the calculation. However there are loads of small ways you can optimize this approach by ensuring you do not perform the algorithm when you are on a node that its current weight is higher than the current lowest solution.

### Summary

In summary the steps to get this solution is:

* Convert the `int[][]` into this node graph and create connections to empty tiles `1` and connections to walls extremely high `MAX_INT`.

* Once you have created the graph calculate the shortest path from the start to the end. (Remember that the start is weight 1 by default according to the spec)

* Once you have calculated the shortest path from `start` to `exit` then calculate shortest paths for each wall and replace the `current_shortest` if the new one calculated is better. (Remember always use the weight you calculated for them in the __FIRST__ calculation but subtract `MAX_INT + 1`)

* After exhausting all our options we will _definitely_ have the shortest solution.

Some tips:

* This approach, if taken how it is will not be very optimized.

* Since you will have a `current_shortest` don't bother calculating nodes that their current weight is > shortest.

* You may wish to use objects to represent nodes and edges.

* Keep the first calculation from `start` to `exit` so that you use the weights of walls you calculated in the first Dijkstra algorithm rather than replacing them with new calculations that you may find when calculating walls. This is because you can only remove __one__ wall and you may accidently use weights that were calculated with a removed wall... Kind of hard to explain but you should get it.

In conclusion this solution is _fairly_ crude but gets the job done and for this use case of a max 10x10 I think it is completely fine to use this kind of algorithm to calculate paths multiple times. Overall I feel that this challenge is not to hard to pass but hard to optimize.
