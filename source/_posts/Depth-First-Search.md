---
title: Depth First Search
date: 2022-02-18
tags: algorithm
---

### <span style="color:red">What is Depth First Search?</span>

Depth First Search (DFS) is an <span style="background-color:yellow">**algorithm**</span> usually used to:

- Traverse all nodes in a graph, tree or 2D array.
- Search if there is a path from the root node to the target node in a graph, tree or 2D array.
- Find all paths from the root node to the target node in a graph, tree or 2D array.

---

### <span style="color:red">Template 1</span>

**Recursion template of DFS to find if there is a path from curr node to target node.**

```java
// initially visited should contain curr node
boolean DFS(Node curr, Node target, Set<Node> visited) {
  if (curr == target) {
    return true;
  }

  for (Node next : curr.neighbors) {
    if (!visited.contains(next)) {
      visited.add(next);
      if (DFS(next, target, visited)) {
        return true;
      }
    }
  }

  return false;
}
```

#### Key Attributes:

- We are using the implicit stack provided by the system, also known as the Call Stack.
- If the depth of recursion is too high, you will suffer from stack overflow.

---

### <span style="color:red">Template 2</span>

**Template of DFS using an explicit stack to find if there is a path from curr node to target node.**

```java
boolean DFS(Node root, Node target) {
  if (root == target) {
    return true;
  }

  Deque<Node> stack = new ArrayDeque<>();
  Set<Node> visited = new HashSet<>();
  stack.push(root);
  visited.add(root);

  while (!stack.isEmpty()) {
    Node curr = stack.pop();
    if (curr == target) {
      return true;
    }
    for (Node next : curr.neighbors) {
      if (!visited.contains(next)) {
        stack.push(next);
        visited.add(next);
      }
    }
  }

  return false;
}
```

#### Key Attributes:

- The logic is exactly the same with the recursion solution. But we use while loop and stack to simulate the system call stack during recursion.

### <span style="color:red">Time and Space Complexity</span>

**Time Complexity: <span style="background-color:yellow">O(V + E)</span>**
V represents the number of vertices, and E represents the number of edges. We need to check every vertex and traverse through every edge in the graph.

**Space Complexity: <span style="background-color:yellow">O(V)</span>**
Either the manually created stack or the recursive call stack can store up to V vertices.

### <span style="color:red">Further Discussion</span>

DFS can be used to find all paths from the source vertex to the target vertex on a directed acyclic graph (DAG)

```java
/**
Given a directed acyclic graph (DAG) of n nodes labeled from 0 to n - 1,
find all possible paths from node 0 to node n - 1 and return them in any order.
graph[i] is a list of all nodes you can visit from node i (i.e., there is a directed edge from node i to node graph[i][j]).
**/
List<List<Integer>> allPathsSourceTarget(int[][] graph) {
  List<List<Integer>> paths = new ArrayList<>();
  dfs(graph, 0, new ArrayList<>(), paths);
  return paths;
}


// this is also regarded as backtrack solution
void dfs(int[][] graph, int node, List<Integer> path, List<List<Integer>> paths) {
  path.add(node);
  if (node == graph.length - 1) {
    paths.add(new ArrayList<>(path));
    return;
  }
  for (int next : graph[node]) {
    dfs(graph, next, path, paths);
    path.remove(path.size() - 1);
  }
}
```

**Time Complexity: <span style="background-color:yellow">O(2<sup>V</sup> · V)</span>**

- V represents the number of vertices.
- For a directed acyclic graph (DAG) with V vertices, there could be at most 2<sup>V - 1</sup> - 1 possible paths to go from the starting vertex to the target vertex. We need O(V) time to build each such path.
- Therefore, a loose upper bound on the time complexity would be O(2<sup>V - 1</sup> - 1) · O(V) = O(2<sup>V</sup> · V).
- Since we have overlap between the paths, the actual time spent on the traversal will be lower to some extent.

**Space Complexity: <span style="background-color:yellow">O(2<sup>V</sup> · V)</span>**

- The recursion depth can be no more than V, and we need O(V) space to store all the previously visited vertices while recursively traversing deeper with the current path.
- since at most we could have 2<sup>V - 1</sup> - 1 possible paths as the results and each path can contain up to V vertices, the space we need to store the results would be O(2<sup>V - 1</sup> - 1) · O(V) = O(2<sup>V</sup> · V).
- To sum up, the space complexity of the algorithm is O(V) + O(2<sup>V - 1</sup> - 1) · O(V) = O(2<sup>V</sup> · V).

**related questions:**

- https://leetcode.com/problems/all-paths-from-source-to-target/

### <span style="color:red">Reference</span>

- https://leetcode.com/explore/learn/card/queue-stack/232/practical-application-stack/
- https://leetcode.com/explore/learn/card/graph/619/depth-first-search-in-graph
