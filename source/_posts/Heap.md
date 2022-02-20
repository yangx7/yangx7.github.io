---
title: Heap
date: 2022-02-20
tags:
---

### <span style="color:red">What is Heap?</span>

Heap is a special type of binary tree <span style="background-color:yellow">**data structure**</span>, it should meets the following criteria:

- It is a complete binary tree. A complete binary tree is a binary tree in which every level, except possibly the last, is completely filled, and all nodes are as far left as possible.
- The value of each node must be no greater than (or no less than) the value of its child nodes.

### <span style="color:red">Classification of Heap</span>

There are two kinds of heaps: Max Heap and Min Heap.

- <span style="background-color:yellow">**Max Heap**</span>: Each node in the Heap has a value no less than its child nodes. Therefore, the top element (root node) has the largest value in the Heap.
- <span style="background-color:yellow">**Min Heap**</span>: Each node in the Heap has a value no larger than its child nodes. Therefore, the top element (root node) has the smallest value in the Heap.
  The figure below shows examples of Max Heap and Min Heap.
  <img src="https://s2.loli.net/2022/02/20/8ComVjrXAI25T4t.png" width="100%">

### <span style="color:red">Transform a complete binary tree into an array</span>

We can implement a heap using an array. Since heap is a complete binary tree, we can store heap elements in the array and get following relationships (Assume the root node has index 1 in the array, its left node has index 2 etc.):

- Given a node has index n in the array, its parent node is at index `n / 2`.
- Given a node has index n in the array, its left child is at index `n * 2`, its right child is at index `n * 2 + 1`.
- Given the total number of nodes in the complete binary tree N, nodes with array index larger than the integer result of `N / 2` is a leaf node.

### <span style="color:red">Implementation of Min Heap</span>

```java
public class MinHeap {
  int[] minHeap;
  int heapSize;
  int realSize;

  public MinHeap(int heapSize) {
    this.minHeap = new int[heapSize + 1];
    this.heapSize = heapSize;
    this.realSize = 0;
  }

  public void add(int element) {
    realSize++;
    if (realSize > heapSize) {
      realSize--;
      return;
    }

    minHeap[realSize] = element;

    int index = realSize;
    int parent = index / 2;
    // if the newly added element is smaller than its parent node,
    // exchange the value of the newly added element with that of its parent
    while (minHeap[index] < minHeap[parent] && index > 1) {
      int temp = minHeap[index];
      minHeap[index] = minHeap[parent];
      minHeap[parent] = temp;
      index = parent;
      parent = index / 2;
    }
  }

  public int peek() {
    return minHeap[1];
  }

  public int pop() {
    if (realSize < 1) {
      return Integer.MAX_VALUE;
    }

    int res = minHeap[1];
    // put the last element in the heap to the top of the heap
    minHeap[1] = minHeap[realSize];
    realSize--;
    int index = 1;
    while (index <= realSize / 2) {
      int left = index * 2;
      int right = index * 2 + 1;
      if (minHeap[index] <= minHeap[left] && minHeap[index] <= minHeap[right]) {
        break;
      }
      /**
      if the parent element is larger than its left or right child,
      its value needs to be exchanged with the smaller value of the left and right child
      **/
      if (minHeap[left] < minHeap[right]) {
        int temp = minHeap[left];
        minHeap[left] = minHeap[index];
        minHeap[index] = temp;
        index = left;
      } else {
        int temp = minHeap[right];
        minHeap[right] = minHeap[index];
        minHeap[index] = temp;
        index = right;
      }
    }

    return res;
  }

  public int size() {
    return realSize;
  }
}
```

### <span style="color:red">Implementation of Max Heap</span>

```java
public class MaxHeap {
  int[] maxHeap;
  int heapSize;
  int realSize;

  public MaxHeap(int heapSize) {
    this.maxHeap = new int[heapSize + 1];
    this.heapSize = heapSize;
    this.realSize = 0;
  }

  public void add(int element) {
    realSize++;
    if (realSize > heapSize) {
      realSize--;
      return;
    }

    maxHeap[realSize] = element;

    int index = realSize;
    int parent = index / 2;
    // if the newly added element is larger than its parent node,
    // exchange the value of the newly added element with that of its parent
    while (maxHeap[index] > maxHeap[parent] && index > 1) {
      int temp = maxHeap[index];
      maxHeap[index] = maxHeap[parent];
      maxHeap[parent] = temp;
      index = parent;
      parent = index / 2;
    }
  }

  public int peek() {
    return maxHeap[1];
  }

  public int pop() {
    if (realSize < 1) {
      return Integer.MIN_VALUE;
    }

    int res = maxHeap[1];
    // put the last element in the heap to the top of the heap
    maxHeap[1] = maxHeap[realSize];
    realSize--;
    int index = 1;
    while (index <= realSize / 2) {
      int left = index * 2;
      int right = index * 2 + 1;
      if (maxHeap[index] >= minHeap[left] && minHeap[index] >= minHeap[right]) {
        break;
      }
      /**
      if the parent element is samller than its left or right child,
      its value needs to be exchanged with the smaller value of the left and right child
      **/
      if (maxHeap[left] > maxHeap[right]) {
        int temp = maxHeap[left];
        maxHeap[left] = maxHeap[index];
        maxHeap[index] = temp;
        index = left;
      } else {
        int temp = maxHeap[right];
        maxHeap[right] = maxHeap[index];
        maxHeap[index] = temp;
        index = right;
      }
    }

    return res;
  }

  public int size() {
    return realSize;
  }
}
```

### <span style="color:red">Time and Space Complexity</span>

| Heap method            | Time complexity | Space complexity |
| ---------------------- | --------------- | ---------------- |
| Construct a Heap       | O(N)            | O(N)             |
| Insert an element      | O(logN)         | O(1)             |
| Get the top element    | O(1)            | O(1)             |
| Delete the top element | O(logN)         | O(1)             |
| Get the size of a Heap | O(1)            | O(1)             |

N is the number of elements in the heap.

**Note: To create a heap, there are two ways:**

- Create an empty heap and repeatly insert elements one by one, this way takes O(NlogN) time.
- Heapify (means converting a group of data into a Heap) directly will take O(N) time.
  Refer to [this](https://leetcode.com/explore/learn/card/heap/644/common-applications-of-heap/4145/) for more information.

### <span style="color:red">Heap Sort</span>

Heap Sort sorts a group of unordered elements using the Heap data structure.

- To sort a group of unordered elements in **ascending order**:
  1. Heapify all elements into a **Min Heap**.
  2. Record and delete the top element.
  3. Put the top element into an array that stores all sorted elements. Now the Heap will remain a Min Heap.
  4. Repeat steps 2 and 3 until the Heap is empty. The array will contain all elements sorted in ascending order.
- To sort a group of unordered elements in **descending order**:
  1. Heapify all elements into a **Max Heap**.
  2. Record and delete the top element.
  3. Put the top element into an array that stores all sorted elements. Now the Heap will remain a Max Heap.
  4. Repeat steps 2 and 3 until the Heap is empty. The array will contain all elements sorted in descending order.

Let N be the total number of elements.
**Time Complexity: <span style="background-color:yellow">O(NlogN)</span>**
**Space Complexity: <span style="background-color:yellow">O(N)</span>**

### <span style="color:red">Priority Queue</span>

- Priority Queue is an <span style="background-color:yellow">**abstract data type**</span> in which each element additionally has a "priority" associated with it. In a priority queue, an element with high priority is served before an element with low priority.
- **A priority queue is an abstract data type, while a Heap is a data structure. Therefore, a Heap is not a Priority Queue, but a way to implement a Priority Queue.**

### <span style="color:red">Further Discussion</span>

Heap is often used in these two class of problems:

- Obtain top k largest or smallest elements.
- Obtain the k-th largest or smallest element.

To obtain **top k largest** elements, we can use either **Max Heap** or **Min Heap**:
Use **Max Heap**:

```java
public int findKthLargest(int[] nums, int k) {
  PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
  for (int num : nums) {
    pq.add(num);
  }
  while (k - 1 > 0) {
    pq.poll();
    k--;
  }
  // pq.peek() is the k-th largest element
  // the first k elements popped out from pq are the top k largest elements
  return pq.peek();
}
```

Use **Min Heap (a more efficient way)**:

```java
public int findKthLargest(int[] nums, int k) {
  PriorityQueue<Integer> pq = new PriorityQueue<>();
  for (int num : nums) {
    pq.add(num);
    if (pq.size() > k) {
      pq.poll();
    }
  }
  // pq.peek() is the k-th largest element
  // pq contains top k largest elements
  return pq.peek();
}
```

To obtain **top k smallest** elements, we can use either **Max Heap** or **Min Heap**:
Use **Min Heap**:

```java
public int findKthSmallest(int[] nums, int k) {
  PriorityQueue<Integer> pq = new PriorityQueue<>();
  for (int num : nums) {
    pq.add(num);
  }
  while (k - 1 > 0) {
    pq.poll();
    k--;
  }
  // pq.peek() is the k-th smallest element
  // the first k elements popped out from pq are the top k smallest elements
  return pq.peek();
}
```

Use **Max Heap (a more efficient way)**:

```java
public int findKthSmallest(int[] nums, int k) {
  PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
  for (int num : nums) {
    pq.add(num);
    if (pq.size() > k) {
      pq.poll();
    }
  }
  // pq.peek() is the k-th smallest element
  // pq contains top k smallest elements
  return pq.peek();
}
```

**related questions:**

- https://leetcode.com/problems/kth-largest-element-in-an-array/
- https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/

### <span style="color:red">Reference</span>

- https://leetcode.com/explore/learn/card/heap/645/applications-of-heap
