                                                                  RUBIKS CUBE SOLVER
Solving Rubik’s Cube using Algorithm 
There are 43,252,003,274,489,856,000 possible states of Rubik’s Cube. Despite that, in 2010 it was shown that any Rubik’s Cube configuration can be solved in 20 moves or fewer.
We can use our Human algorithm (the way humans solve the cube) and program it. Our program will solve it within milliseconds. But it will probably do 100s of moves to solve the cube. What we are interested in is to solve the cube in a very small number of moves. And we want the program to output those moves/operations done to solve the cube. 
Therefore, what we can do is we can define one particular configuration of Rubik’s Cube as a state and we can transition to other states by doing some operations. This structure is very similar to a Graph.
A set of graph algorithms that we can use are:
Depth-First Search (DFS)
Breadth-First Search (BFS)
Iterative-Deepening Depth-First Search (IDDFS)

All these algorithms are very similar. All of them: visit a node, mark the node visited, and then proceed to visit its neighbours in some order. We can use unordered_map<RubiksCubeClass, bool>.This unordered_map will map the entire Rubik’s Cube states to a boolean value.

****DFS SOLVER:****
We don’t want to write 3 different solvers for the 3 representations. We want the generic solver to take in the Rubik’s Cube in any representation along with it’s Hash function.
This solver will take the initial Rubik’s cube and do a DFS until it finds the solver state of the cube. But with DFS the problem is it can keep going into depth and may find a very long solution by taking infinite time. But the fact is rubiks cube can be solved in 20 moves or fewer.
So I implemented the DFS Solver which take Rubiks Cube State and max_depth and upon calling the Solve() function it will solve the rubiks cube and return the number of steps it required to solve it. Making the solver genric for all 3 representation( which are 3Darray , 1DArray and bit-borad representation) buy making use of template class.
But there is a caveat in this solver If we implemented this and try to do it for different amount of random shuffle and max depth we found out it does not give a solution even if max_depth > random shuffle.
It turns out we are using two types of restrictions that are causing this problem. We are trying to limit the amount of exploration by not visiting the already visited nodes. Also, we are restricting the solver to not go beyond the depths of 20. 
To deal with this problem we can remove the visited map. We would allow our solver to revisit nodes. This will increase the complexity of our solver, but it will then at least be able to solve the Rubik’s Cube. 

**BFS SOLVER**
Next Comes the BFS Solver.A BFS Solver will work very similarly to a DFS Solver,just using the BFS strategy to search the solution space. Since, BFS is an optimal solver, that is it is going to find the minimum number of moves needed to reach the Goal state, we don’t need to explicitly specify the max search depth. It Will Solve the Rubiks Cube in less than or equal to 20 moves.


**IDDFS:(Iterative Deepening Depth First Search):**

I Implemented 2 Solver which are BFS and DFS solvers. But these two are not very efficient when it comes to traversing very large and very deep graphs.
The Problem with DFS is that it is taking the much longer time than expected and may not find the optimal solution and in BFS which goes level by level by require more space. The Space Complexity is O(n) where n=no of nodes.

**IDDFS combines depth-first search’s space-efficiency and breadth-first search’s fast search**
IDDFS:
https://en.wikipedia.org/wiki/Iterative_deepening_depth-first_search
<img width="318" height="213" alt="IDA*" src="https://github.com/user-attachments/assets/bd6705ab-0131-4a27-9109-060e01b22485" />

Iterative deepening prevents this loop and will reach the following nodes on the following depths, assuming it proceeds left-to-right as above:
    •  Depth 0: A
    •  Depth 1: A,B,C,E
      (Iterative deepening has now seen C, when a conventional depth-first search did not.)
    •  Depth 2: A,B,D,F,C,G,E,F
(It still sees C, but that it came later. Also it sees E via a different path, and loops back to F twice.)
    •  Depth 3: A,B,D,F,E,C,G,E,F,B



**IDA*Solver Korf’s Algorithm:**

The next Solver is IDA* star using Korf’s Algorithm. Korf, in his research paper, introduces an Algorithm to solve the Rubik’s Cube in optimal number of moves using the IDA* Algorithm which combines the A* algorithm with IDDFS and uses pattern databases as a heuristic.

**A*Algorithm:**

A* algorithm is graph traversal algorithm , similar to Dijkstra’s algorithm, the only difference being that it uses guided search. In Dijkstra’s, we used to explore the nodes which were closest (minimum weight) to the source but we didn’t have any idea if we were going in the right direction or not. 

This is where A* differs. It uses a metric (called heuristic) to get an estimate of how far the current node is from the goal state.
This heuristic need not be exact but can be an underestimate of the actual distance to the goal state. It combines this heuristic with the distance travelled from the source to the current node, and prioritises nodes which have the sum of heuristic + distance travelled as minimum. 
A* prioritizes the nodes which have minimum f(n), thus, exploring them first.

**Heuristic:**

Heuristic is nothing but an estimate (or a guess) of how far a particular node is from the goal node. This can be the exact distance or an underestimate. A*algorithm uses this estimate/guess to find the optimal path from the source node to the goal node. Now, this heuristic can be pre-computed. The way we get this heuristic can be different for different problems. In our case, we create pattern databases to use as a heuristic.

**Pattern Databases:**

Pattern Database is probably the most important part of this algorithm. A Pattern Database will store the number of moves it will take to reach the solved/goal state of the Rubik’s cube.
This is where we can create Corner Pattern Database and Edge Pattern Database, as suggested by Korf in his research paper. 

**Corner Pattern Database: **

This database will contain the number of moves required to solve the corners of the Rubik’s Cube, that is, assume that beside the 8 corners, all other cubbies are blank (or solved) at all times, and then solve the Rubik’s cube.
IDA* Algorithm:
IDA* is the actual algorithm that we use to create the solver, as suggested by Korf. This algorithm is just a combination of A* algorithm and IDDFS algorithm. 

**Corner Pattern Database:**
There are 8 corner cubbies, and each cubie can acquire any of the 8 positions, giving us 8! different possible permutations. Each of the corner cubie can be oriented in 3 different directions - any of three stickers facing up. But the orientation of the 7 cubies dictate the orientation of the 8th cubie. 
This gives us a total of 8! * 3^7 possible ways to jumble the corners of Rubik’s cube. These 88,179,840 states can be then iterated upon (do a BFS). Another fact that we can use here is that all these states are solvable in 11 moves or fewer. Thus, the answer of these states can be stored in a nibble (4 bits). This gives a total size of 88 million * 4/8 = 42 MB of data. This gives us a neat way to store the answers (heuristic values) for different cube configuration.



**Corner Pattern Database using Permutation Indexer:**

Now to implement the Corner Pattern database. We will inherit the pattern database class to implement this class.
Then implemented getDatabaseIndex() method. This method should return the calculated index of a given Rubik’s Cube.
Permutation Indexer takes in an array of numbers (permutation) and returns it’s corresponding index. In our case, we first need to get the rank/index of the corner permutations then combine it with corner orientations to get the actual rank/index.
Index = (index_of_permutations * (3^7)) + index_of_orientations
Index and orientation of a corner are nothing but a number from 0→7 and 0→2 respectively.
Implement the member functions in the base class of Rubik’s Cube that would give the index and orientation of the corner:
    • getCornerIndex(): Given a Rubik’s cube and a corner, find which corner is placed there (0, 1, 2, 3, 4, 5, 6, 7).
    • getCornerOrientation(): Given a Rubik’s cube and a corner, find the orientation of that corner (0, 1, 2)
      In a Solved Rubiks Cube(white facing up and red facing front) 
      there are different permutation of corner which we can get along with the orientation.
    • RWG→1
    • OWG→2
    • WBO→3
    • WBR→4 YRB→5  YRG→6 YRO→7  YOB→8
      Then there are 3 orientation 0→2 for three dfifferent colors which are there.<img width="449" height="430" alt="ModelofCube" src="https://github.com/user-attachments/assets/0421440d-67e8-4c49-aaab-c72eed14ff29" />


Implemented the CornerPatternDatabase class and its getDatabaseIndex() method by combining the ranks of corner index permutation and corner orientation. Used the PermutationIndexer class as a black box.
Now the whole implementation is done the only thing left is to actually do BFS and store the value in file. After the Database is created we can now make changes in our IDA*  Algorithm to give the real estimate which works as a heuristic.
Learning:
I learned how to take a real world object, like a Rubik’s Cube, and model it into something that the computer can understand.
I learned how to take a larger problem, like the complete Rubik’s Cube solver, and break it down into smaller problems, using basic models and controllers, that is, Rubik’s cube representations and solvers.
I learned how path finding algorithms like BFS, DFS, IDDFS and IDA* can be used to solve real world problems related to AI. Many more problems in the field of AI can be solved using these algorithms.
I learned about the concept of Heuristic Search Algorithms like A* and IDA* and how Pattern Databases can be created to provide that heuristic.



















