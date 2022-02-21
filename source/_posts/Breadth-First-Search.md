---
title: Breadth First Search
date: 2022-02-18
tags: algorithm
---

### <span style="color:red">What is Breadth First Search?</span>

Breadth First Search (BFS) is an <span style="background-color:yellow">**algorithm**</span> usually used to：

- Traverse all nodes level by level in a graph, tree or 2D array.
- Find the shortest path from the root node to the target node in a graph, tree or 2D array.

---

### <span style="color:red">Template 1</span>

**Most basic and elementary form of BFS to traverse all nodes in a Binary Tree.**

```java
List<Node> BFS(Node root) {
  List<Node> ans = new ArrayList<>();

  Queue<Node> queue = new LinkedList<>();
  queue.offer(root);

  while (!queue.isEmpty()) {
    Node curr = queue.poll();
    ans.add(curr);
    if (curr.left != null) {
      queue.offer(curr.left);
    }
    if (curr.right != null) {
      queue.offer(curr.right);
    }
  }

  return ans;
}
```

#### Key Attributes:

- Since we only want to traverse all nodes in a Binary Tree, we do not need to worry about detecting cycles or care about different levels of the tree, just add left and right children of the current node to the queue if they are not null.

---

### <span style="color:red">Template 2</span>

**Find the shortest path from the root node to the target node in a tree or a directed acyclic graph (DAG).**

```java
int BFS(Node root, Node target) {
  Queue<Node> queue = new LinkedList<>();
  queue.offer(root);

  int step = 0;
  while (!queue.isEmpty()) {
    int size = queue.size();
    for (int i = 0; i < size; i++) {
      Node curr = queue.poll();
      if (curr == target) {
        return step;
      }
      /**
      Add children nodes of a tree or neighbors in a graph to the queue
      e.g. If we are traversing a Binary Tree,
      we should add curr.left and curr.right to the queue if they are not null
      **/
      for (Node next : curr.neighbors) {
        queue.offer(next);
      }
    }
    step++;
  }

  // Return -1 if there is no path from root to target
  return -1;
}
```

#### Key Attributes:

- As shown in the code, in each round, we need to keep the current queue size, nodes inside this range are from the current level we are working on.
- After each outer while loop, we are one step farther from the root node. The variable step indicates the distance from the root node and the current node we are visiting.
- There is a variant of increasing step, instead of updating step at the end of the while loop, we can also increasing step at the beginning of the while loop:

```java
int step = 0;
while (!queue.isEmpty()) {
  step++;
  int size = queue.size();
  for (int i = 0; i < size; i++) {
    Node curr = queue.poll();
    for (Node next : curr.neighbors) {
      if (next == target) {
        return step;
      }
      queue.offer(next);
    }
  }
}
```

In this way, we can terminate the finding process even earlier, but we need to be carefull about handling the initial root node firstly before we go to the while loop.

---

### <span style="color:red">Template 3</span>

**If we need to make sure that we never visit a node twice or want to avoid getting stuck in an infinite loop in a graph with cycle, we can keep a set to contain all node we have visited.**

```java
int BFS(Node root, Node target) {
  Queue<Node> queue = new LinkedList<>();
  Set<Node> visited = new HashSet<>();

  queue.offer(root);
  visited.add(root);

  int step = 0;
  while (!queue.isEmpty()) {
    int size = queue.size();
    for (int i = 0; i < size; i++) {
      Node curr = queue.poll();
      if (curr == target) {
        return step;
      }
      for (Node next : curr.neighbors) {
        if (!visited.contains(next)) {
          queue.offer(next);
          visited.add(next);
        }
      }
    }
    step++;
  }

  return -1;
}
```

#### Key Attributes:

- Key difference between this template and template 2 is that here we keep a set to contain all visited nodes, before we add a new node to the queue, we need to check if the node is already in the set.
- This template is used if you are given a graph that may have cycles or you do not want to visit a node multiple times like traversing in an 2D array.

### <span style="color:red">Time and Space Complexity</span>

|                      | Binary Tree/ N-ary Tree                               | Graph                                                     | 2D Array                                                   |
| -------------------- | ----------------------------------------------------- | --------------------------------------------------------- | ---------------------------------------------------------- |
| **Time Complexity:** | <span style="background-color:yellow">**O(N)**</span> | <span style="background-color:yellow">**O(V + E)**</span> | <span style="background-color:yellow">**O(M \* N)**</span> |

- In the Binary Tree or N-ary Tree, N represents the number of nodes, the number of edges will be exactly N - 1, so together the time complexity is O(N).
- In the graph, V represents the number of vertices, and E represents the number of edges. We need to check every vertex and traverse through every edge in the graph.
- In the 2D array of size M \* N, we need to traverse all elements in the matrix, so the time complexity is O(M \* N).

|                       | Binary Tree/ N-ary Tree                               | Graph                                                 | 2D Array                                                   |
| --------------------- | ----------------------------------------------------- | ----------------------------------------------------- | ---------------------------------------------------------- |
| **Space Complexity:** | <span style="background-color:yellow">**O(N)**</span> | <span style="background-color:yellow">**O(V)**</span> | <span style="background-color:yellow">**O(M \* N)**</span> |

- In the Binary Tree or N-ary Tree, at worst case we need to hold all nodes in the queue.
- In the graph, we will check if a vertex has been visited before adding it to the queue, so the queue will use at most O(V) space. Keeping track of which vertices have been visited will also require O(V) space.
- In the 2D array, at worst case we need to hold all elements in the queue. Keeping track of which elements have been visited will also require O(M \* N) space.

### <span style="color:red">Further Discussion</span>

Template 3 is also widely used in searching a 2D Array. The question may define that we can search 4 directions from one point, that is up, down, left and right. In this case, we can predefine a direction array and use it when we want to generate new points.

**Find the shortest path from the start point to the target point in a 2D array.**

```java
int BFS(int[][] mat, int[] start, int[] target) {
  int m = mat.length;
  int n = mat[0].length;

  Queue<int[]> queue = new LinkedList<>();
  boolean[][] visited = new boolean[m][n];

  queue.offer(start);
  visited[start[0]][start[1]] = true;

  // define directions vector in clockwise direction: up, right, down, left
  int[][] DIRS = {{-1, 0}, {0, 1}, {1, 0}, {0, -1}};

  int step = 0;
  while (!queue.isEmpty()) {
    int size = queue.size();
    for (int i = 0; i < size; i++) {
      int[] curr = queue.poll();
      int row = curr[0];
      int col = curr[1];
      if (row == target[0] && col == target[1]) {
        return step;
      }
      for (int[] dir : DIRS) {
        int r = row + dir[0];
        int c = col + dir[1];
        if (r < 0 || r >= m || c < 0 || c >= n || visited[r][c]) {
          continue;
        }
        queue.offer(new int[]{r, c});
        visited[r][c] = true;
      }
    }
    step++;
  }

  return -1;
}
```

### <span style="color:red">Reference</span>

- https://leetcode.com/explore/learn/card/queue-stack/231/practical-application-queue/
- https://leetcode.com/explore/learn/card/graph/620/breadth-first-search-in-graph/3883/
