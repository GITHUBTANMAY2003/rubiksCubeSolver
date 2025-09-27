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

<img width="1197" height="898" alt="Screenshot from 2025-09-22 15-29-13" src="https://github.com/user-attachments/assets/95f490b1-87bb-4c59-b53a-c4f3223ed4fe" />


