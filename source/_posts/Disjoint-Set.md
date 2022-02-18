---
title: Disjoint Set
date: 2022-02-18
tags:
---

### <span style="color:red">What is Disjoint Set?</span>

- Disjoint Set is a <span style="background-color:yellow">**data structure**</span> usually applied in a graph, also known as Union Find data structure.
- The primary use of Disjoint Set is to address the connectivity between the components of a network. The "network" here can be a computer network or a social network. For instance, we can use a Disjoint Set to determine if two people share a common ancestor.
- The main idea of Disjoint Set is to have all connected vertices have the same root node, whether directly or indirectly connected. In this way if we want to check two vertices are connected, we only need to check if they have the same root node.

### <span style="color:red">Two essential functions of Disjoint Set</span>

- <span style="background-color:yellow">**Find**</span> function finds the root node of a given vertex.
- <span style="background-color:yellow">**Union**</span> function connects previously unconnected two vertices by making their root nodes the same.
- There is another important function named **connected**, which checks the connectivity of two vertices.

### <span style="color:red">Two basic ways to implement Disjoint Set</span>

- **implementation with <span style="background-color:yellow">Quick Find</span>**: The time complexity of the find function will be O(1), the union function will take more time with the time complexity of O(N).
- **implementation with <span style="background-color:yellow">Quick Union</span>**: Compared with the quick find implementation, the find function will take more time in this case wiht the time complexity of O(N), the time complexity of the union function is also O(N), but performs better than the union functon of Quick Find implementation.

### <span style="color:red">Two common strategies to optimize Disjoint Set</span>

- <span style="background-color:yellow">**Path Compression**</span>: This optimization is after finding the root node of a given vertex, we update the parent node of all traversed vertices to their root node. When we search for the root node of the same vertex again, we only need to traverse two vertices to find its root node, which is highly efficient. The time complexity of find function is reduced from O(n) to O(logn).
- <span style="background-color:yellow">**Union by Rank**</span>: For the union function, instead of always picking the root of the first vertex as the new root node, we choose the root node of the vertex with a larger "rank". We will merge the shorter tree under the taller tree and assign the root node of the taller tree as the root node for both vertices. In this way, we effectively avoid the possibility of connecting all vertices into a straight line. The time complexity of union function is reduced from O(n) to O(logn).

### <span style="color:red">Optimized Disjoint Set with path compression and union by rank</span>

```java
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
```

### <span style="color:red">Time and Space Complexity</span>

|                      | Union-find Constructor                                | Find                                                     | Union                                                    | Connected                                                |
| -------------------- | ----------------------------------------------------- | -------------------------------------------------------- | -------------------------------------------------------- | -------------------------------------------------------- |
| **Time Complexity:** | <span style="background-color:yellow">**O(N)**</span> | <span style="background-color:yellow">**O(α(N))**</span> | <span style="background-color:yellow">**O(α(N))**</span> | <span style="background-color:yellow">**O(α(N))**</span> |

N is the number of vertices in the graph. α refers to the Inverse Ackermann function. In practice, we assume it is a constant. In other words, O(α(N)) is regarded as O(1) on average.

**Space Complexity: <span style="background-color:yellow">O(N)</span>**
We need two arrays of size N to record root nodes and rank of root nodes.

### <span style="color:red">Further Discussion</span>

In some problems, we are required to return the number of different groups or some extra information like the size of the maximum group. To get these information, we can keep some variables and update them while union two vertices.

```java
class UnionFind {
  private int[] root;
  private int[] rank;
  // returns the number of different groups
  public int groups;
  // returns the size of the largest group
  public int maxGroupSize;

  public UnionFind(int size) {
    root = new int[size];
    rank = new int[size];
    for (int i = 0; i < size; i++) {
      root[i] = i;
      // here rank array keeps the size of each group
      rank[i] = 1;
    }
    // initially all vertices are independent groups of size 1
    groups = size;
    // initially all groups have one element
    maxGroupSize = 1;
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

    if (rank[rootX] >= rank[rootY]) {
      root[rootY] = rootX;
      rank[rootX] += rank[rootY];
    } else {
      root[rootX] = rootY;
      rank[rootY] += rank[rootX];
    }

    // each valid union operation will reduce the number of groups by one
    groups--;
    // update maxGroupSize in each valid union
    maxGroupSize = Math.max(maxGroupSize, Math.max(rank[rootX], rank[rootY]));
  }

  public boolean connected(int x, int y) {
    return find(x) == find(y);
  }
}
```

**related questions:**

- https://leetcode.com/problems/groups-of-strings/

### <span style="color:red">Reference</span>

- https://leetcode.com/explore/learn/card/graph/618/disjoint-set
