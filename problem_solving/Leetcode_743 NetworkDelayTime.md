# LeetCode 743 - Network Delay Time
---
## Probleam Summary :   
You are given a directed, weighted graph with n nodes labeled(1...n).  
Each edge times[i] = (u, v, w) represents a directed edge from u to v taking w time.  

A signal start from node k  
Return the time it takes for all nodes to recieve the signal.  
If any node is unreachable, return -1.  

---
## Key Insight:  
This is a single-source shortest path problem on a weighted directed graph.  
The final answer is the maximum of all shortest distances from K.  
Since all edge weights are non-negtive, ```Dijkstra``` algorithm is the natural fit.  

---
## Approach ：Dijkstra [Algorithm Note](../AlgorithmNote/Graph/Dijsktra.md)
