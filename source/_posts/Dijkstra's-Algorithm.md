---
title: Dijkstra's Algorithm
date: 2022-02-19
tags:
---

### <span style="color:red">What is Dijkstra's Algorithm?</span>

- Dijkstra’s algorithm is an <span style="background-color:yellow">**algorithm**</span> used to solve the "single-source shortest path" problem in a **weighted directed graph with non-negative weights**.
- "single source shortest path" problems: Given the starting vertex, find the shortest path to any of the vertices in a weighted graph. Once we solve this, we can easily acquire the shortest paths between the starting vertex and a given target vertex.
- Note the difference between unweighted graphs and weighted graphs, below left is an unweighted graph, every edge has the same weight/distance; right is a weighted graph, every edge may have different weight/distance.
  | Unweighted Graph | Weighted Graph |
  | --- | --- |
  | <img src="https://s2.loli.net/2022/02/20/Glpw74LOD3Un9MI.jpg" width="60%" align="left"> | <img src="https://s2.loli.net/2022/02/20/iDa85eU6QdbC2pc.jpg" width="60%" align="left"> |

### <span style="color:red">Implement Dijkstra's Algorithm</span>

Main idea is taking the starting point as the center and gradually expand outward while updating the shortest path to reach other vertices. Dijkstra's Algorithm uses a "greedy approach". Each step selects the "minimum weight" from the currently reached vertices to find the shortest path to other vertices.

**Algorithm**:

1.  For all vertices, initialize the paths table with a large value, so far, no vertex has been visited.
2.  Initialize min heap with the pair of startNode and its distance 0, store its distance in paths as 0. While the min heap is not empty:
    - Pop out the currNode from the min heap.
    - Traverse all outgoing edges connected to currNode.
    - Add the neighborNode to the min heap only if the current path is less than the value at paths[neighborNode]. Update its value to current path value.
3.  Find the value paths[targetNode], if the value is still the large value we initialized the array with, it means the targetNode is not reachable from the startNode, return -1. Otherwise, return the value we get in the array.

```java
/**
We are give a graph with n nodes labeled from 0 to n - 1.
An array edges listing all directed edges of a graph.
edges[i] = (ui, vi, wi), where ui is the source node, vi is the target node, wi is the weight it takes from ui to vi.
we need to find the shortest path costing minimum weight to reach the target node from the source node.
**/
int Dijkstra(int n, int[][] edges, int source, int target) {
  // Create the adjacency list of the graph
  Map<Integer, List<int[]>> adj = new HashMap<>();
  for (int i = 0; i < n; i++) {
    adj.put(i, new ArrayList<>());
  }

  for (int[] edge : edges) {
    adj.get(edge[0]).add(new int[]{edge[1], edge[2]});
  }

  int[] paths = new int[n];
  Arrays.fill(paths, Integer.MAX_VALUE);
  paths[source] = 0;

  PriorityQueue<int[]> pq = new PriorityQueue<>((v1, v2) -> v1[1] - v2[1]);
  pq.add(new int[]{source, 0});

  while (!pq.isEmpty()) {
    int[] curr = pq.poll();
    int currNode = curr[0];
    int weight = curr[1];
    if (weight > paths[currNode]) {
      continue;
    }
    for (int[] next : adj.get(currNode)) {
      int nextNode = next[0];
      int nextWeight = next[1];
      if (weight + nextWeight < paths[nextNode]) {
        paths[nextNode] = weight + nextWeight;
        pq.add(new int[]{nextNode, paths[nextNode]});
      }
    }
  }

  return paths[target] == Integer.MAX_VALUE ? -1 : paths[target];
}
```

**related questions:**

- https://leetcode.com/problems/network-delay-time/

### <span style="color:red">Time and Space Complexity</span>

**Time Complexity: <span style="background-color:yellow">O(V + ElogE)</span>**

- V represents the number of vertices, and E represents the number of edges. The value of E can be at most `V · (V - 1)`.
- Build the adjacency list takes O(V + E) time.
- The maximum number of pairs that could be added to the priority queue is E. Thus, push and pop operations on the priority queue take O(logE) time.
- Although the number of vertices in the priority queue could be equal to E, we will only visit each vertex only once. In total E edges will be traversed, and for each edge, there could be one priority queue insertion operation. This takes O(ElogE) time.
- Therefore, the overall time complexity is `O(V + E) + O(ElogE) = O(V + ElogE)`.

**Space Complexity: <span style="background-color:yellow">O(V + E)</span>**

- Building the adjacency list will take O(E) space.
- Keep the paths array takes O(V) space.
- Each vertex could be added to the priority queue `V - 1` times, so in total priority queue takes `O(V · (V - 1)) = O(E)` space.
- Therefore, the overall space complexity is `O(V) + O(E) = O(V + E)`.

### <span style="color:red">Reference</span>

- https://leetcode.com/explore/learn/card/graph/622/single-source-shortest-path-algorithm/3862/
