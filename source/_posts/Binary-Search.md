---
title: Binary Search
date: 2022-02-17
tags:
---

### <span style="color:red">What is Binary Search?</span>

Binary Search is an <span style="background-color:yellow">**algorithm**</span> that divides the search space in 2 after every comparison. Binary Search should be considered every time you need to search for an index or element in a collection. If the collection is unordered, we can always sort it first before applying Binary Search.

### <span style="color:red">3 Parts of a Successful Binary Search</span>

Binary Search is generally composed of 3 main sections:

- <span style="background-color:yellow">**Pre-processing**</span>: Sort if collection is unsorted.
- <span style="background-color:yellow">**Binary Search**</span>: Using a loop or recursion to divide search space in half after each comparison.
- <span style="background-color:yellow">**Post-processing**</span>: Determine viable candidates in the remaining space.

---

### <span style="color:red">Template 1</span>

```java
int BinarySearch(int[] nums, int target) {
  if (nums == null || nums.length == 0) {
    return -1;
  }

  int left = 0;
  int right = nums.length - 1;

  while (left <= right) {
    int mid = left + (right - left) / 2;
    if (nums[mid] == target) {
      return mid;
    }
    if (nums[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }

  return -1;
}
```

Template 1 is the most basic and elementary form of Binary Search. It is used to search for an element or condition which can be determined by accessing a single index in the array.

#### Distinguishing Syntax:

- Initial Condition: <span style="background-color:yellow">**left = 0, right = length - 1**</span>
- Termination: <span style="background-color:yellow">**left > right**</span>
- Searching Left: <span style="background-color:yellow">**right = mid - 1**</span>
- Searching Right: <span style="background-color:yellow">**left = mid + 1**</span>

#### Key Attributes:

- Most basic and elementary form of Binary Search.
- Search condition can be determined without comparing to the element's neighbors.
- No post-processing required because at each step, you are checking to see if the element has been found. If you reach the end, then you know the element is not found.

---

### <span style="color:red">Template 2</span>

```java
int BinarySearch(int[] nums, int target) {
  if (nums == null || nums.length == 0) {
    return -1;
  }

  int left = 0;
  int right = nums.length;

  while (left < right) {
    int mid = left + (right - left) / 2;
    if (nums[mid] == target) {
      return mid;
    }
    if (nums[mid] < target) {
      left = mid + 1;
    } else {
      right = mid;
    }
  }

  if (left != nums.length && nums[left] == target) {
    return left;
  }

  return -1;
}
```

Template 2 is an advanced form of Binary Search. It is used to search for an element or condition which requires accessing the current index and its immediate right neighbor's index in the array.

#### Distinguishing Syntax:

- Initial Condition: <span style="background-color:yellow">**left = 0, right = length**</span>
- Termination: <span style="background-color:yellow">**left == right**</span>
- Searching Left: <span style="background-color:yellow">**right = mid**</span>
- Searching Right: <span style="background-color:yellow">**left = mid + 1**</span>

#### Key Attributes:

- An advanced way to implement Binary Search.
- Search condition needs to access element's immediate right neighbor.
- Use element's right neighbor to determine if condition is met and decide whether to go left or right.
- Gurantee search space is at least 2 in size at each step.
- Post-processing required. Loop/Recursion ends when you have 1 element left. Need to assess if the remaining element meets the condition.

---

### <span style="color:red">Template 3</span>

```java
int BinarySearch(int[] nums, int target) {
  if (nums == null || nums.length == 0) {
    return -1;
  }

  int left = 0;
  int right = nums.length - 1;

  while (left + 1 < right) {
    int mid = left + (right - left) / 2;
    if (nums[mid] == target) {
      return mid;
    }
    if (nums[mid] < target) {
      left = mid;
    } else {
      right = mid;
    }
  }

  if (nums[left] == target) {
    return left;
  }

  if (nums[right] == target) {
    return right;
  }

  return -1;
}
```

Template 3 is another unique form of Binary Search. It is used to search for an element or condition which requires accessing the current index and its immediate left and right neighbor's index in the array.

#### Distinguishing Syntax:

- Initial Condition: <span style="background-color:yellow">**left = 0, right = length - 1**</span>
- Termination: <span style="background-color:yellow">**left + 1 == right**</span>
- Searching Left: <span style="background-color:yellow">**right = mid**</span>
- Searching Right: <span style="background-color:yellow">**left = mid**</span>

#### Key Attributes:

- An alternative way to implement Binary Search.
- Search condition needs to access element's immediate left and right neighbors.
- Use element's neighbors to determine if condition is met and decide whether to go left or right.
- Gurantee search space is at least 3 in size at each step.
- Post-processing required. Loop/Recursion ends when you have 2 elements left. Need to assess if the remaining elements meet the condition.

---

### <span style="color:red">Time and Space Complexity</span>

**Time Complexity: <span style="background-color:yellow">O(logN)</span>**
Binary Search operates by applying a condition to the value in the middle of the search space and thus cutting the search space in half, in the worst case, we will have to make O(logN) comparisons, where N is the number of elements in our input collection.

**Space Complexity: <span style="background-color:yellow">O(1)</span>**
Although, Binary Search does require keeping track of 3 indicies, the iterative solution does not typically require any other additional space and can be applied directly on the collection itself, therefore warrants O(1) or constant space.

### <span style="color:red">Further Discussion</span>

Template 2 is widely used in one class of questions, that is to find the minimum or maximum possible value that meets a certain condition.

To apply template 2, the question should satisfy these requirements:

- The question has limited number of consecutive candidates.
- The candidates can be divided into exactly two parts, the first consecutive part all meets the condition and no candidate from the second part meets the condition, or vice versa.

The main idea is to gradually reach the extreme value required in the question. In each step, we need to make sure that the current value (int mid = left + (right - left) / 2) meets the condition. In the last step, the condition is not meet, then the second to last value is the value we want.

**related questions:**

- https://leetcode.com/problems/longest-repeating-substring/
- https://leetcode.com/problems/koko-eating-bananas/

### <span style="color:red">Reference</span>

- https://leetcode.com/explore/learn/card/binary-search/
