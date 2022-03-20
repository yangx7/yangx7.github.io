---
title: Segment Tree
date: 2022-03-19
tags: algorithm
---

### <span style="color:red">What is Segment Tree?</span>

Segment tree is a special type of binary tree <span style="background-color:yellow">**data structure**</span>, where each node represents an interval. Generally a node would store one or more properties of an interval which can be queried later. Many problems require that we give results based on query over a range or segment of available data. This can be a tedious and slow process, especially if the number of queries is large and repetitive. A segment tree let us process such queries efficiently in logarithmic order of time.

### <span style="color:red">Basic Ideas of Segment Tree</span>

Let our data be in an array `data` of size n.

- The root of segment tree typically represents the entire interval of data. This would be `[data[0], data[n - 1]]`.
- Each leaf of the tree represents a range comprising of just a single element. Thus the leaves represent `data[0]`, `data[1]` and so on till `data[n - 1]`.
- The internal nodes of the tree would represent the merged result of their children nodes.
- Each of the children nodes could represent approximately half of the range represented by their parent.

### <span style="color:red">Template 1: Recursive method for Segment Tree</span>

```java
class SegmentTree {
  private int n;
  private int[] tree;

  public SegmentTree(int[] data) {
    n = data.length;
    // note our segment tree is 1-indexed, tree[1] is the root of the tree
    tree = new int[4 * n];
    buildTree(data, 0, 0, n - 1);
  }

  /**
  merge operation varies from problem to problem.
  public int merge(int leftVal, int rightVal) {
  }
  **/

  public void buildTree(int[] data, int treeIndex, int left, int right) {
    if (left == right) {
      tree[treeIndex] = data[left];
      return;
    }

    int mid = left + (right - left) / 2;
    buildTree(data, 2 * treeIndex, left, mid);
    buildTree(data, 2 * treeIndex + 1, mid + 1, right);

    tree[treeIndex] = merge(tree[2 * treeIndex], tree[2 * treeIndex + 1]);
  }

  // here [i, j] is the range/interval we are quering
  public int queryTree(int treeIndex, int left, int right, int i, int j) {
    // segment completely outside range returns 0
    if (left > j || right < i) {
      return 0;
    }

    // segment completely inside range returns tree[treeIndex]
    if (i <= left && j >= right) {
      return tree[treeIndex];
    }

    int mid = left + (right - left) / 2;
    if (mid < i) {
      // quering range/interval is inside right half of the segment
      return queryTree(2 * treeIndex + 1, mid + 1, right, i, j);
    } else if (mid >= j) {
      // quering range/interval is inside left half of the segment
      return queryTree(2 * treeIndex, left, mid, i, j);
    }

    int leftQuey = queryTree(2 * treeIndex, left, mid, i, mid);
    int rightQuery = queryTree(2 * treeIndex + 1, mid + 1, right, mid + 1, j);

    return merge(leftQuery, rightQuery);
  }

  // update the value of data[dataIndex] with new value val
  public void updateTree(int treeIndex, int left, int right, int dataIndex, int val) {
    if (left == right) {
      tree[treeIndex] = val;
      return;
    }

    int mid = left + (right - left) / 2;
    if (mid < dataIndex) {
      updateTree(2 * treeIndex + 1, mid + 1, right, dataIndex, val);
    } else {
      updateTree(2 * treeIndex, left, mid, dataIndex, val);
    }

    tree[treeIndex] = merge(tree[2 * treeIndex], tree[2 * treeIndex + 1]);
  }
}
```

#### Key Attributes:

- Here we initialize the tree with a int array of size `4 * n`, note that a segment tree for an n element range can be comfortably represented using an array of size `n ≈ 4 ∗ n`. This ensures that we build our segment tree as a complete binary tree, which in turn ensures that the height of the tree is upper-bounded by the logarithm of the size of our input. Also, we can initialize the tree array with other data types based on the conditions and requirements of problems.
- The merge method varies from problem to problem, it can return the sum or other information of the interval.
- Here nodes of the tree is 1-indexed, thus tree[1] is the root of our tree. The children of tree[i] are stored at tree[2 * i] and tree[2 * i + 1]. If nodes are 0-indexed, then tree[0] is the root, the children of tree[i] are stored at tree[2 * i + 1] and tree[2 * i + 2].

### <span style="color:red">Time and Space Complexity</span>

|                      | constructor/buildTree                                 | queryTree                                                | updateTree                                               |
| -------------------- | ----------------------------------------------------- | -------------------------------------------------------- | -------------------------------------------------------- |
| **Time Complexity:** | <span style="background-color:yellow">**O(N)**</span> | <span style="background-color:yellow">**O(logN)**</span> | <span style="background-color:yellow">**O(logN)**</span> |

- buildTree: We visit each leaf of the segment tree (corresponding to each element in our array data). That makes N leaves, thus there will be N - 1 internal nodes. So we process 2 \* N nodes. This makes the build process run in O(N) linear complexity.
- queryTree: At best, we query for the entire range and get our result from the root of the segment tree itself. At worst, we query for a interval/range of size 1 (which corresponds to a single element), and we end up traversing through the height of the tree. This takes time linear to height of the tree. Therefore time complexity is O(logN).
- updateTree: After the leaf is updated, its direct ancestors at each level of the tree are updated. This takes time linear to height of the tree. Therefore time complexity is O(logN).

**Space Complexity: <span style="background-color:yellow">O(N)</span>**
We use an array of size `4 * N` to store all elements of data in a complete binary tree array.

**related questions:**

- https://leetcode.com/problems/range-sum-query-mutable/
- https://leetcode.com/problems/longest-substring-of-one-repeating-character/

### <span style="color:red">Reference</span>

- https://leetcode.com/articles/a-recursive-approach-to-segment-trees-range-sum-queries-lazy-propagation/
