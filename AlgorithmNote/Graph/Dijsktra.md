# Dijkstra's Algorithm Notes
## What Problem Does Dijkstra Solve?
Dijkstra algorithm solves the **Single-Source Shortest Path (SSSP)** problem.
> Given a graph with non-negtive edge weights, it computes the shortest distance from a single source node to all other nodes.

✅ Directed Graph  
✅ Undirected Graph  
❗️ Edge weights must be non-negtive  

## Core Idea / Algorithm Paradigm
**Greedy algorithm + BFS**
1. select the unvisited node with the smallest known distance from the source.  
2. Mark this spot has been visited
3. Update all unvisited spot distance

