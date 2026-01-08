# Dijkstra's Algorithm Notes
## What Problem Does Dijkstra Solve?
Dijkstra algorithm solves the **Single-Source Shortest Path (SSSP)** problem.
> Given a graph with non-negtive edge weights, it computes the shortest distance from a single source node to all other nodes.

✅ Directed Graph  
✅ Undirected Graph  
❗️ Edge weights must be non-negtive  
---
## Core Idea / Algorithm Paradigm
**Greedy algorithm + BFS**
1. select the unvisited node with the smallest known distance from the source.  
2. Mark this spot has been visited
3. Update all unvisited spot distance

## Algorithm Steps (High-Level)
1. initialize:
   - dist[source] = 0
   - All other distance = ∞
2. Push (0, source) into a min-priority queue
3. While the priority queue is not empty:
   - Extract node u with the smallest dist
   - For each outgoing edge(u -> v, weight):
     - If dist[u] + weight < dist[v]
         - update dist[v]
         - push (dist[v], v) into the queue
4. distances array contains the shortest paths
