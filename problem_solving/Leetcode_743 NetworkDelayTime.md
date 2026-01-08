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
1. Build adj list for graph  
```java
/**
  Input : [[2,1,1],[2,3,1],[3,4,1]], n = 4, k =2
  Build Graph : 
    Node | <Neighbor, Weight>
------------------------------
    2    | <1, 1> <3, 1>
    3    | <4, 1>
    Graph : List<int[]>[] 
*/
```  
2. initialize dis[] with infinity, set dist[k] = 0, means k to k start with 0 cost  
3. Use min-heap to always expand the shortest known node
4. After Dijkstra finishes:  
   - If any node is still infinity -> unreachable -> return -1
   - Otherwise return max(dist[])
---  
## Why not BFS?
BFS works only when all edge weights are equal

---
## Code
```java
class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        List<int[]>[] graph = new LinkedList[n];
        for (int i = 0; i < n; i++) {
            graph[i] = new LinkedList<>();
        }
        for (int[] t : times) {
            int start = t[0] - 1;
            int to = t[1] - 1;
            int weight = t[2];
            graph[start].add(new int[]{to, weight});
        }
        int[] distToK = dijkstra(graph, k - 1);
        int maxDist = 0;
        for (int di : distToK) {
            if (di == -1) return -1;
            maxDist = Math.max(maxDist, di);
        }
        return maxDist;
    }

    private int[] dijkstra(List<int[]>[] graph, int start) {
        int[] distTo = new int[graph.length];
        Arrays.fill(distTo, -1);
        int[] startNode = new int[]{start, 0};
        PriorityQueue<int[]> pq = new PriorityQueue<>((a,b) -> {
            return a[1] - b[1];
        });
        pq.offer(startNode);
        while (!pq.isEmpty()) {
            int[] curNode = pq.poll();
            int curNodeNum = curNode[0];
            int curDist = curNode[1];

            if (distTo[curNodeNum] != -1 && distTo[curNodeNum] < curDist) continue;
            distTo[curNodeNum] = curDist;
            for (int[] nei : graph[curNodeNum]) {
                int nextNode = nei[0];
                int nextDist = nei[1] + curDist;
                if (distTo[nextNode] == -1 || nextDist < distTo[nextNode]) {
                    distTo[nextNode] = nextDist;
                    pq.offer(new int[]{nextNode, nextDist});
                }
            }
        }
        return distTo;
    }
}
```
