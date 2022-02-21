---
title: Topological Sorting
date: 2022-02-20
tags: algorithm
---

### <span style="color:red">What is Topological Sorting?</span>

Topological sorting provides a linear sorting based on the required ordering between vertices in a directed acyclic graph(DAG). To be specific, given vertices u and v, to reach vertex v, we must have reached vertex u first. In topological sorting, u has to appear before v in the ordering. The most popular algorithm for topological sorting is Kahn’s algorithm.

### <span style="color:red">Kahn’s algorithm</span>

```java
/**
We are give a graph with n nodes labeled from 0 to n - 1.
An array prerequisites, prerequisites[i] = [ui, vi] indicates that you must reach vi first if you want to reach ui,
return the ordering of vertices you should take to visit all vertices
**/
int[] Kahn(int n, int[][] prerequisites) {
  Map<Integer, List<Integer>> adj = new HashMap<>();
  for (int i = 0; i < n; i++) {
    adj.put(i, new ArrayList<>());
  }

  int[] indegree = new int[n];
  for (int[] pre : prerequisites) {
    adj.get(pre[1]).add(pre[0]);
    indegree[pre[0]]++;
  }

  Queue<Integer> queue = new LinkedList<>();
  for (int i = 0; i < n; i++) {
    if (indegree[i] == 0) {
      queue.offer(i);
    }
  }

  int[] ans = new int[n];
  int index = 0;
  while (!queue.isEmpty()) {
    int curr = queue.poll();
    ans[index++] = curr;
    for (int next : adj.get(curr)) {
      indegree[next]--;
      if (indegree[next] == 0) {
        queue.offer(next);
      }
    }
  }

  if (index != n) {
    // if there is no valid topological sorting, return an empty array
    return new int[0];
  }

  return ans;
}
```

#### Key Attributes:

- Topological sorting only works with directed acyclic graph(DAG).
- There must be at least one vertex in the graph with an indegree of 0. If all vertices in the graph have a non-zero indegree, then all vertices need at least one vertex as a predecessor. In this case, no vertex can serve as the starting vertex.

**related questions:**

- https://leetcode.com/problems/course-schedule-ii/

### <span style="color:red">Time and Space Complexity</span>

**Time Complexity: <span style="background-color:yellow">O(V + E)</span>**

- V represents the number of vertices, and E represents the number of edges.
- Building the adjacency list will take O(E) time, as we must iterate over all edges.
- We will repeatedly visit each vertex with an indegree of zero and decrement the indegree of all vertices that have this vertex as a prerequisite. In the worst case, we will visit every vertex and decrement every outgoing edge once. This part will take O(V + E) time.
- Therefore, the total time complexity is `O(E) + O(V + E) = O(V + E)`.

**Space Complexity: <span style="background-color:yellow">O(V + E)</span>**

- Building the adjacency list will take O(E) space.
- Storing the indegree for each vertex requires O(V) space.
- The queue can contain at most V vertices, so the queue also requires O(V) space.
- Therefore, the total time complexity is `O(E) + O(V) = O(V + E)`.

### <span style="color:red">Reference</span>

- https://leetcode.com/explore/learn/card/graph/623/kahns-algorithm-for-topological-sorting
