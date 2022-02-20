---
title: Minimum Spanning Tree
date: 2022-02-19
tags:
---

### <span style="color:red">What is Minimum Spanning Tree?</span>

A **Spanning Tree** is a connected subgraph in an undirected graph where all vertices are connected with the minimum number of edges.
In the below figure, all pink edges <span style="background-color:yellow">[(A, B), (A, C), (A, D), (A, E)]</span> form a tree, which is a spanning tree of this undirected graph. Note that <span style="background-color:yellow">[(A, E), (A, B), (B, C), (C, D)]</span> is also a spanning tree of the undirected graph. **Thus, an undirected graph can have multiple spanning trees**.
<img src="https://s2.loli.net/2022/02/19/J7AEUHv8MX5qZlL.png" width="60%">

A **Minimum Spanning Tree (MST)** is a spanning tree with the minimum possible total edge weight in a weighted undirected graph.
In the below figure, a spanning tree formed by green edges <span style="background-color:yellow">[(A, E), (A, B), (B, C), (C, D)]</span> is one of the minimum spanning trees in this weighted undirected graph. Actually, <span style="background-color:yellow">[(A, E), (E, D), (A, B), (B, C)]</span> forms another minimum spanning tree of the weighted undirected graph. **Thus, a weighted undirected graph can have multiple minimum spanning trees**.
<img src="https://s2.loli.net/2022/02/19/Sbumhe7NR9EBnw4.png" width="60%">

### <span style="color:red">Cut Property</span>

- In Graph theory, a "cut" is a partition of vertices in a graph into two disjoint subsets. Below figure illustrates a "cut", where (B, A, E) forms one subset, and (C, D) forms the other subset.
- A crossing edge is an edge that connects a vertex in one set with a vertex in the other set. In the below figure, (B, C), (A, C), (A, D), (E, D) are all "crossing edges".
  <img src="https://s2.loli.net/2022/02/19/IZTp5qkSXb746Mz.png" width="60%">

<span style="background-color:yellow">**For any cut C of the graph, if the weight of an edge E in the cut-set of C is strictly smaller than the weights of all other edges of the cut-set of C, then this edge belongs to all MSTs of the graph.**</span>

So in the above figure, (B, C) belongs to all MSTs of the graph. The cut property provides theoretical support for the two algorithms for constructing a minimum spanning tree: Kruskal’s algorithm and Prim’s algorithm.

### <span style="color:red">Kruskal’s Algorithm</span>

Kruskal’s algorithm is an <span style="background-color:yellow">**algorithm**</span> to construct a MST of a weighted undirected graph.

**Algorithm**:

1.  Ascending sort all edges by their weight. In practice, we can use min heap to get the edge wih the smallest weight.
2.  Add edges in ascending order into the MST. Skip the edges that would produce cycles in the MST.
3.  Repeat step 2 until `N - 1` edges are added.

```java
class Solution {
  /**
  Given an array points representing integer coordinates of some points on a 2D-plane,
  where points[i] = [xi, yi].
  The cost of connecting two points [xi, yi] and [xj, yj] is the manhattan distance between them:
  |xi - xj| + |yi - yj|, where |val| denotes the absolute value of val.
  **/
  public int Kruskal(int[][] points) {
    if (points == null || points.length == 0) {
      return 0;
    }

    int size = points.length;
    UnionFind uf = new UnionFind(size);
    PriorityQueue<Edge> pq = new PriorityQueue<>((p1, p2) -> p1.cost - p2.cost);

    for (int i = 0; i < size; i++) {
      int[] p1 = points[i];
      for (int j = i + 1; j < size; j++) {
        int[] p2 = points[j];
        int cost = Math.abs(p1[0] - p2[0]) + Math.abs(p1[1] - p2[1]);
        Edge edge = new Edge(i, j, cost);
        pq.add(edge);
      }
    }

    int ans = 0;
    // to construct MST, we need (size - 1) edges
    int count = size - 1;
    while (!pq.isEmpty() && count > 0) {
      Edge edge = pq.poll();
      if (!uf.connected(edge.point1, edge.point2)) {
        uf.union(edge.point1, edge.point2);
        ans += edge.cost;
        count--;
      }
    }

    return ans;
  }

  class Edge {
    int point1;
    int point2;
    int cost;

    public Edge(int point1, int point2, int cost) {
      this.point1 = point1;
      this.point2 = point2;
      this.cost = cost;
    }
  }

  class UnionFind {
    private int[] root;
    private int[] rank;

    public UnionFind(int size) {
      root = new int[size];
      rank = new int[size];
      for (int i = 0; i < size; i++) {
        root[i] = i;
        rank[i] = 1;
      }
    }

    public int find(int x) {
      if (x == root[x]) {
        return x;
      }
      return root[x] = find(root[x]);
    }

    public void union(int x, int y) {
      int rootX = find(x);
      int rootY = find(y);
      if (rootX == rootY) {
        return;
      }

      if (rank[rootX] > rank[rootY]) {
        root[rootY] = rootX;
      } else if (rank[rootX] < rank[rootY]) {
        root[rootX] = rootY;
      } else {
        root[rootY] = rootX;
        rank[rootX]++;
      }
    }

    public boolean connected(int x, int y) {
      return find(x) == find(y);
    }
  }
}
```

**Time Complexity: <span style="background-color:yellow">O(ElogE)</span>**

- V represents the number of vertices, E represents the total number of edges. In our template, `E = V · (V - 1)`.
- Building a priority queue takes O(ElogE) time.
- To construct MST using Kruskal's algorithm, We pop out each edge from the priority queue, and for each edge, we will look at whether both of the vertices of the edge belong to the same connected component and connect them if they do not belong to the same connected component. We only need `V - 1` edges to build MST, but in the worst case, the tree will not be complete until we reach the very last edge whose weight is the largest, it takes O(E) iterations. So it totally takes O(ElogE) time to pop out all edges from priority queue, to check connectivity and union vertices in the Union Find data structure totally takes O(Eα(V)) time, we can regard it as O(E) time.
- Therefore, in total, the time complexity is `O(ElogE) + O(ElogE) + O(E) = O(ElogE)` time.

**Space Complexity: <span style="background-color:yellow">O(V + E)</span>**

- E represents the number of edges. Store all edges in a priority queue takes O(E) sapce.
- V represents the number of vertices, Using the Union Find data structure requires O(V) space.
- Therefore, in total, the space complexity is `O(V) + O(E) = O(V + E)` time.

### <span style="color:red">Prim's Algorithm</span>

Prim's algorithm is an <span style="background-color:yellow">**algorithm**</span> to construct a MST of a weighted undirected graph.

**Algorithm**:

1.  Keep two sets to store vertices: non-visited and visited. Initially, all vertices are in the non-visited set.
2.  Randomly pick one vertex from non-visited set, put it in the visited set.
3.  Among all edges between visited and non-visited vertices, find the edge with the smallest weight.
4.  Move the new non-visited vertex belonging to the edge we found at step 3 to the visited set.
5.  Repeat step 2 and 3 until all vertices are added to the visited set.

```java
class Solution {
  /**
  Given an array points representing integer coordinates of some points on a 2D-plane,
  where points[i] = [xi, yi].
  The cost of connecting two points [xi, yi] and [xj, yj] is the manhattan distance between them:
  |xi - xj| + |yi - yj|, where |val| denotes the absolute value of val.
  **/
  public int Prim(int[][] points) {
    if (points == null || points.length == 0) {
      return 0;
    }

    int size = points.length;
    boolean[] visited = new boolean[size];
    PriorityQueue<Edge> pq = new PriorityQueue<>((p1, p2) -> p1.cost - p2.cost);

    // select the first point to be added to the visited set
    int[] p1 = points[0];
    visited[0] = true;
    for (int i = 1; i < size; i++) {
      int[] p2 = points[i];
      int cost = Math.abs(p1[0] - p2[0]) + Math.abs(p1[1] - p2[1]);
      Edge edge = new Edge(0, i, cost);
      pq.add(edge);
    }

    int ans = 0;
    // we need to add remaining (size - 1) vertices to the visited set
    int count = size - 1;
    while (!pq.isEmpty() && count > 0) {
      Edge edge = pq.poll();
      int p2 = edge.point2;
      if (!visited[p2]) {
        visited[p2] = true;
        ans += edge.cost;
        count--;
        for (int i = 0; i < size; i++) {
          if (!visited[i]) {
            int cost = Math.abs(points[p2][0] - points[i][0]) + Math.abs(points[p2][1] - points[i][0]);
            pq.add(new Edge(p2, i, cost));
          }
        }
      }
    }

    return ans;
  }

  class Edge {
    int point1;
    int point2;
    int cost;

    public Edge(int point1, int point2, int cost) {
      this.point1 = point1;
      this.point2 = point2;
      this.cost = cost;
    }
  }
}
```

**Time Complexity: <span style="background-color:yellow">O(ElogV)</span>**

- V represents the number of vertices, E represents the total number of edges. In our template, `E = V · (V - 1)`.
- We need O(V + E) time to traverse all the vertices of the graph, and we store in the heap all the vertices that are not yet included in our MST.
- Pop out vertices from priority queue takes O(logV) time.
- Therefore, the overall time complexity is `O(V + E) · O(log V) = O(ElogV)`.

**Space Complexity: <span style="background-color:yellow">O(V)</span>**

- V represents the number of vertices, Store all edges stating from one point in a priority queue takes O(V) sapce.

### <span style="color:red">Reference</span>

- https://leetcode.com/explore/learn/card/graph/621/algorithms-to-construct-minimum-spanning-tree
