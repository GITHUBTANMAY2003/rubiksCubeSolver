Solving Rubik’s Cube using Algorithm 

    • There are 43,252,003,274,489,856,000 possible states of Rubik’s Cube. Despite that, in 2010 it was shown that any Rubik’s Cube configuration can be solved in 20 moves or fewer.
    • We can use our Human algorithm (the way humans solve the cube) and program it. Our program will solve it within milliseconds. But it will probably do 100s of moves to solve the cube. What we are interested in is to solve the cube in a very small number of moves. And we want the program to output those moves/operations done to solve the cube. 
    • Therefore, what we can do is we can define one particular configuration of Rubik’s Cube as a state and we can transition to other states by doing some operations. This structure is very similar to a Graph.
      
    • A set of graph algorithms that we can use are:
    1. Depth-First Search (DFS)
    2. Breadth-First Search (BFS)
    3. Iterative-Deepening Depth-First Search (IDDFS)
All these algorithms are very similar. All of them: visit a node, mark the node visited, and then proceed to visit its neighbours in some order. We can use unordered_map<RubiksCubeClass, bool>.This unordered_map will map the entire Rubik’s Cube states to a boolean value.

DFS SOLVER:

    • We don’t want to write 3 different solvers for the 3 representations. We want the generic solver to take in the Rubik’s Cube in any representation along with it’s Hash function.
    • 
    • This solver will take the initial Rubik’s cube and do a DFS until it finds the solver state of the cube. But with DFS the problem is it can keep going into depth and may find a very long solution by taking infinite time. But the fact is rubiks cube can be solved in 20 moves or fewer.
