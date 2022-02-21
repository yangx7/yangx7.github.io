---
title: Monotonic Stack
date: 2022-02-20
tags: algorithm
---

### <span style="color:red">What is Monotonic Stack?</span>

- A Monotonic Stack is a stack with its elements ordered monotonically from stack bottom to top. So we have two kinds of monotonic stacks: **monotonic increase stack** and **monotonic decrease stack**.
- Monotonic Stack usually works as a <span style="background-color:yellow">**technique**</span> to resolve problems that requires finding the next/previous greater/smaller element of each element in a collection.
- To obtain **previous/next samller element**, use **monotonic increase stack**.
- To obtain **previous/next greater element**, use **monotonic decrease stack**.

### <span style="color:red">Basic Template for monotonic increase stack</span>

**Get previous smaller element:**

```java
/**
Given an array nums, return a new array ans,
where ans[i] contains the previous samller element of nums[i] in the nums array.
**/
int[] previousSmallerElement(int[] nums) {
  int n = nums.length;
  Deque<Integer> stack = new ArrayDeque<>();
  int[] ans = new int[n];

  for (int i = 0; i < n; i++) {
    // stack is a monotonic increase stack
    while (!stack.isEmpty() && nums[stack.peek()] > nums[i]) {
      stack.pop();
    }
    ans[i] = stack.isEmpty() ? -1 : nums[stack.peek()];
    // store index instead of the value in the stack
    stack.push(i);
  }

  return ans;
}
```

**Get next smaller element:**

```java
/**
Given an array nums, return a new array ans,
where ans[i] contains the next samller element of nums[i] in the nums array.
**/
int[] nextSmallerElement(int[] nums) {
  int n = nums.length;
  Deque<Integer> stack = new ArrayDeque<>();
  int[] ans = new int[n];

  for (int i = 0; i < n; i++) {
    // stack is a monotonic increase stack
    while (!stack.isEmpty() && nums[stack.peek()] > nums[i]) {
      ans[stack.pop()] = nums[i];
    }
    // store index instead of the value in the stack
    stack.push(i);
  }

  // if there is no next smaller element for some element, return -1
  while (!stack.isEmpty()) {
    ans[stack.pop()] = -1;
  }

  return ans;
}
```

#### Key Attributes:

- To keep the stack monotonic increasing, once we find the peek element of the stack is larger than the current element to be pushed to the stack, pop out the current peek element from the stack.
- We push every element of the nums array to the stack once.
- Since it is easy to access the value by index, we usually store index instead of the value in the stack.
- To get previous smaller element, update ans[i] outside the while loop since current stack.peek() is less than nums[i].
- To get next smaller element, update ans[i] inside the while loop since each stack.peek() is greater than nums[i].

### <span style="color:red">Basic Template for monotonic decrease stack</span>

**Get previous greater element:**

```java
/**
Given an array nums, return a new array ans,
where ans[i] contains the previous greater element of nums[i] in the nums array.
**/
int[] previousGreaterElement(int[] nums) {
  int n = nums.length;
  Deque<Integer> stack = new ArrayDeque<>();
  int[] ans = new int[n];

  for (int i = 0; i < n; i++) {
    // stack is a monotonic decrease stack
    while (!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
      stack.pop();
    }
    ans[i] = stack.isEmpty() ? -1 : nums[stack.peek()];
    // store index instead of the value in the stack
    stack.push(i);
  }

  return ans;
}
```

**Get next greater element:**

```java
/**
Given an array nums, return a new array ans,
where ans[i] contains the next greater element of nums[i] in the nums array.
**/
int[] nextGreaterElement(int[] nums) {
  int n = nums.length;
  Deque<Integer> stack = new ArrayDeque<>();
  int[] ans = new int[n];

  for (int i = 0; i < n; i++) {
    // stack is a monotonic decrease stack
    while (!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
      ans[stack.pop()] = nums[i];
    }
    // store index instead of the value in the stack
    stack.push(i);
  }

  // if there is no next greater element for some element, return -1
  while (!stack.isEmpty()) {
    ans[stack.pop()] = -1;
  }

  return ans;
}
```

#### Key Attributes:

- Similar to monotonic increase stack, to keep the stack monotonic decreasing, once we find the peek element of the stack is samller than the current element to be pushed to the stack, pop out the current peek element from the stack.
- To get previous greater element, update ans[i] outside the while loop since current stack.peek() is greater than nums[i].
- To get next greater element, update ans[i] inside the while loop since each stack.peek() is less than nums[i].

**related questions:**

- https://leetcode.com/problems/next-greater-element-i/

### <span style="color:red">Time and Space Complexity</span>

**Time Complexity: <span style="background-color:yellow">O(N)</span>**
N is the size of the given array. We traverse the array, push and pop every element from the array once, so it takes O(N) time.

**Space Complexity: <span style="background-color:yellow">O(N)</span>**
We keep a stack and in worst case, it stores all elements of the given array, so space complexity is O(N).

### <span style="color:red">Reference</span>

- https://leetcode.com/problems/sum-of-subarray-minimums/discuss/178876/stack-solution-with-very-detailed-explanation-step-by-step
