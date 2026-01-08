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

## Java Template Code (AMC mode)
```java
import java.util.*;

// Online Java Compiler
// Copy-paste and run

public class Main {

    // -------- State --------
    static class State {
        int node;
        long distFromStart; // tentative distance from src to node

        State(int node, long distFromStart) {
            this.node = node;
            this.distFromStart = distFromStart;
        }
    }

    // -------- Edge --------
    static class Edge {
        int to;
        int weight;
        Edge(int to, int weight) {
            this.to = to;
            this.weight = weight;
        }
    }

    // -------- Graph (Adjacency List) --------
    static class Graph {
        List<Edge>[] adj;

        @SuppressWarnings("unchecked")
        Graph(int n) {
            adj = new ArrayList[n];
            for (int i = 0; i < n; i++) adj[i] = new ArrayList<>();
        }

        int size() { return adj.length; }
        List<Edge> neighbors(int u) { return adj[u]; }

        // directed edge
        void addEdge(int from, int to, int weight) {
            adj[from].add(new Edge(to, weight));
        }

        // undirected edge helper
        void addUndirectedEdge(int u, int v, int weight) {
            addEdge(u, v, weight);
            addEdge(v, u, weight);
        }
    }

    // -------- Dijkstra Template --------
    static class DijkstraTemplate {
        // return distTo[]: shortest distance from src to every node
        // unreachable nodes will remain Long.MAX_VALUE
        static long[] dijkstra(Graph graph, int src) {
            long[] distTo = new long[graph.size()];
            Arrays.fill(distTo, Long.MAX_VALUE);

            PriorityQueue<State> pq = new PriorityQueue<>(
                (a, b) -> Long.compare(a.distFromStart, b.distFromStart)
            );

            distTo[src] = 0L;
            pq.offer(new State(src, 0L));

            while (!pq.isEmpty()) {
                State cur = pq.poll();
                int u = cur.node;
                long d = cur.distFromStart;

                // outdated entry check
                if (d > distTo[u]) continue;

                for (Edge e : graph.neighbors(u)) {
                    int v = e.to;
                    long nd = d + e.weight;
                    if (nd < distTo[v]) {
                        distTo[v] = nd;
                        pq.offer(new State(v, nd));
                    }
                }
            }

            return distTo;
        }
    }

    // -------- Helpers --------
    static void printDist(long[] dist) {
        for (int i = 0; i < dist.length; i++) {
            String val = (dist[i] == Long.MAX_VALUE) ? "INF" : String.valueOf(dist[i]);
            System.out.println("dist[" + i + "] = " + val);
        }
    }

    // -------- Main Tests --------
    public static void main(String[] args) {

        // Test 1: Classic shortest path improvement (0 -> 1 direct is worse than 0->2->1)
        // 0->1 (10), 0->2 (1), 2->1 (1), 1->3 (2), 2->3 (10)
        // Expected from src=0: dist[0]=0, dist[2]=1, dist[1]=2, dist[3]=4
        System.out.println("=== Test 1: Directed graph (classic) ===");
        Graph g1 = new Graph(4);
        g1.addEdge(0, 1, 10);
        g1.addEdge(0, 2, 1);
        g1.addEdge(2, 1, 1);
        g1.addEdge(1, 3, 2);
        g1.addEdge(2, 3, 10);

        long[] dist1 = DijkstraTemplate.dijkstra(g1, 0);
        printDist(dist1);
        System.out.println();

        // Test 2: Unreachable node
        // 0->1 (5), 1->2 (7), node 3 isolated
        // Expected: dist[3]=INF
        System.out.println("=== Test 2: Unreachable node ===");
        Graph g2 = new Graph(4);
        g2.addEdge(0, 1, 5);
        g2.addEdge(1, 2, 7);

        long[] dist2 = DijkstraTemplate.dijkstra(g2, 0);
        printDist(dist2);
        System.out.println();

        // Test 3: Undirected graph
        // 0--1 (4), 0--2 (1), 2--1 (2), 1--3 (1), 2--3 (5)
        // Expected from src=0: dist[2]=1, dist[1]=3, dist[3]=4
        System.out.println("=== Test 3: Undirected graph ===");
        Graph g3 = new Graph(4);
        g3.addUndirectedEdge(0, 1, 4);
        g3.addUndirectedEdge(0, 2, 1);
        g3.addUndirectedEdge(2, 1, 2);
        g3.addUndirectedEdge(1, 3, 1);
        g3.addUndirectedEdge(2, 3, 5);

        long[] dist3 = DijkstraTemplate.dijkstra(g3, 0);
        printDist(dist3);
    }
}
```
