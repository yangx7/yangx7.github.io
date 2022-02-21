---
title: Bellman Ford Algorithm
date: 2022-02-19
tags: algorithm
---

### <span style="color:red">What is Bellman Ford Algorithm?</span>

- Bellman Ford Algorithm is an <span style="background-color:yellow">**algorithm**</span> used to solve the "single-source shortest path" problem in a **weighted directed graph with negative weights**.
- "single source shortest path" problem is explained in the [Dijkstra's-Algorithm](../Dijkstra's-Algorithm/) post.
- Edge Relaxation: it is the operation to find a shorter distance path from the source to the target node and update current record.

### <span style="color:red">Two Basic Theorems to support Bellman Ford Algorithm</span>

1. In a "graph with no negative-weight cycles" with N vertices, the shortest path between any two vertices has at most `N - 1` edges. Thus, Bellman Ford Algorithm can detect whether there exists a "negative weight cycle" in the graph.
   **Detection method**: After relaxing each edge `N - 1` times, perform the Nth relaxation. According to the Bellman Ford algorithm, all distances must be the shortest after relaxing each edge `N - 1` times. However, after the Nth relaxation, if there exists distances[u] + weight(u, v) < distances(v) for any edge(u, v), it means there is a shorter path. At this point, we can conclude that there exists a "negative weight cycle".
2. In a "graph with negative weight cycles", there is no shortest path.

---

### <span style="color:red">Template 1</span>

**Implement Bellman Ford algorithm with Dynamic Programming if we have the constraint that at most k edges are allowed from the source vertex to target vertex. If we don't have this constraint, k is actually equals to `N - 1`, because based on theorem 1, the shortest path between any two vertices has at most `N - 1` edges.**

**Algorithm**:

1.  Initialize two arrays prev and curr with a large value, set the source node value to 0.
2.  Traverse all edges(u, v) and update the curr array curr[v] by picking the minimum between curr[v] and `prev[u] + weight`.
3.  Repeat step 2 until K iterations are done.
4.  If curr[target] is still the large value we initialized the array with, it means the target is not reachable from the source, return -1. Otherwise, return curr[target].

```java
/**
We are give a graph with n nodes labeled from 0 to n - 1.
An array edges listing all directed edges of a graph.
edges[i] = (ui, vi, wi), where ui is the source node, vi is the target node, wi is the weight it takes from ui to vi.
we need to find the shortest path having at most k edges to reach the target node from the source node.
**/
int Bellman-Ford(int n, int[][] edges, int source, int target, int k) {
  if (source == target) {
    return 0;
  }

  // after k iterations, prev array keeps the shortest path from souce to any vertices if at most k - 1 edges are allowed
  int[] prev = new int[n];
  // after k iterations, curr array keeps the shortest path from souce to any vertices if at most k edges are allowed
  int[] curr = new int[n];
  Arrays.fill(prev, Integer.MAX_VALUE);
  Arrays.fill(curr, Integer.MAX_VALUE);
  prev[source] = 0;
  curr[source] = 0;

  for (int i = 0; i < k; i++) {
    for (int[] edge : edges) {
      int from = edge[0];
      int to = edge[1];
      int weight = edge[2];
      if (prev[from] < Integer.MAX_VALUE) {
        curr[to] = Math.min(curr[to], prev[from] + weight);
      }
    }
    prev = curr.clone();
  }

  return curr[target] == Integer.MAX_VALUE ? -1 : curr[target];
}
```

**Time Complexity: <span style="background-color:yellow">O(k · E)</span>**
we have k iterations and in each iteration, we go over all the edges in the graph.

**Space Complexity: <span style="background-color:yellow">O(V)</span>**
V represents the number of vertices, we keep two arrays of length V.

**related questions:**

- https://leetcode.com/problems/cheapest-flights-within-k-stops/

---

### <span style="color:red">Template 2</span>

**Implement Bellman Ford algorithm with "the Shortest Path Faster Algorithm" (SPFA algorithm). Note here we do not have the constraint that at most k edges are allowed from the source vertex to target vertex.**

**Algorithm**:

1.  Initialize an array with a large value to keep the shortest path from the source to other vertices, set source vertex value to 0.
2.  Initialize a queue and put the source to the queue.
3.  Pop out the head from the queue, traverse all edges starting from the head and update the shortest path array, if any vertex value is changed, put it back to the queue since it may also impact other vertices value in the shortest path array.
4.  Repeat step 3 and 4 until the queue is empty.
5.  If arr[target] is still the large value we initialized the array with, it means the target is not reachable from the source, return -1. Otherwise, return arr[target].

```java
/**
We are give a graph with n nodes labeled from 0 to n - 1.
An array edges listing all directed edges of a graph.
edges[i] = (ui, vi, wi), where ui is the source node, vi is the target node, wi is the weight it takes from ui to vi.
we need to find the shortest path costing minimum weight to reach the target node from the source node.
**/
int Bellman-Ford(int n, int[][] edges, int source, int target) {
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

  Queue<Integer> queue = new LinkedList<>();
  queue.offer(source);

  while (!queue.isEmpty()) {
    int curr = queue.poll();
    for (int[] edge : adj.get(curr)) {
      int next = edge[0];
      int weight = edge[1];
      if (paths[curr] + weight < paths[next]) {
        paths[next] = paths[curr] + weight;
        queue.offer(next);
      }
    }
  }

  return paths[target] == Integer.MAX_VALUE ? -1 : paths[target];
}
```

**Time Complexity: <span style="background-color:yellow">O(V · E)</span>**

- V represents the number of vertices, and E represents the number of edges.
- Build the adjacency list takes O(V + E) time.
- We iterate through all the vertices, and in each iteration, we'll perform a relaxation operation for each appropriate edge. Therefore, the time complexity would be O(V · E).
- Therefore, the overall time complexity is `O(V + E) + O(V · E) = O(V · E)`.
- On average, the SPFA tends to be faster than the template 1 if there is no constraint of at most k edges are allowed.

**Space Complexity: <span style="background-color:yellow">O(V + E)</span>**

- Building the adjacency list will take O(E) space.
- Keep the paths array takes O(V) space.
- The queue keeps at most V vertices, it takes O(V) space.
- Therefore, the overall space complexity is `O(V) + O(E) = O(V + E)`.

**related questions:**

- https://leetcode.com/problems/path-with-minimum-effort/

### <span style="color:red">Difference between BFS, Dijkstra's Algorithm and Bellman Ford Algorithm in finding shortest path</span>

- BFS can find the shortest path from the source to the target in a tree, 2D array or unweighted graph.
- Dijkstra's Algorithm can find the shortest path in the weighted directed graph with non-negative weights.
- Bellman Ford Algorithm can find the shortest path in the weighted directed graph with negative weights, so it can cover cases of Dijkstra's Algorithm.

### <span style="color:red">Reference</span>

- https://leetcode.com/explore/learn/card/graph/622/single-source-shortest-path-algorithm/3864/
