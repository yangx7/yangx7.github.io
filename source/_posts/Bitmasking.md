---
title: Bitmasking
date: 2022-03-16
tags: algorithm
---

### <span style="color:red">What is Bitmasking?</span>

- Bitmasking is a <span style="background-color:yellow">**technique**</span> to represent some state in a finite set. It can be used to represent a subset of a finite set, one state of a 0-1 2D matrix, etc. For example, consider the set A = {1, 2, 3, 4, 5}, we can represent any subset of A using a binary bitmask of length 5, with the assumption that if ith (0 <= i <= 4) bit is set then it means ith element is present in the subset. So the bitmask 00011 represents the subset {1, 2}.
- In essence, bitmask is just a number that represents some state, the most common case is that the number is 2-based, so each bit can represent at most 2 different states. But the number is not necessarily always a binary number, it can be 3-based, 10-based depending on the question, so each bit can represent more than 2 states.
- Bitmasking is usually combined with other techniques such as Dynamic Programming, BFS, etc.

### <span style="color:red">Three Basic Operations of Binary Bitmask</span>

- <span style="background-color:yellow">**Set the ith bit**</span>

```java
// set the ith bit to 1 (counting i from right to left, 0-indexed)
bitmask |= (1 << i);

/**
For example:
  Set A = {1, 2, 3, 4, 5}.
  bitmask = 01010 represents the subset {2, 4}.
  Let i = 0, set the ith bit, so,
    1 << i = 00001
    01010 | 00001 = 01011
  Therefore currently bitmask represents the subset {1, 2, 4}.
**/
```

- <span style="background-color:yellow">**Unset the ith bit**</span>

```java
// unset the ith bit to 0 (counting i from right to left, 0-indexed)
bitmask &= (~(1 << i));

/**
For example:
  Set A = {1, 2, 3, 4, 5}.
  bitmask = 01010 represents the subset {2, 4}.
  Let i = 1, unset the ith bit, so,
    1 << i = 00010
    ~(1 << i) = 11101
    01010 & 11101 = 01000
  Therefore currently bitmask represents the subset {4}.
**/
```

- <span style="background-color:yellow">**Check if the ith bit is set**</span>

```java
// check if the ith bit is set to 1 (counting i from right to left, 0-indexed)
(bitmask & (1 << i)) > 0

/**
For example:
  Set A = {1, 2, 3, 4, 5}.
  bitmask = 01010 represents the subset {2, 4}.
  Let i = 1, check if the ith bit is set to 1, so,
    1 << i = 00010
    01010 & 00010 = 00010
  Since the result 00010 is greater than 0, the ith bit is set to 1
**/
```

### <span style="color:red">Three Basic Operations of Non-Binary Bitmask</span>

If in one bit we can contain more than 2 states, we need a bitmask with higher base. For example, if each case can contain at most 2 items, then we need a 3-based bitmask number to represent each state. Each bit can have the value 0, 1, 2 representing no items, one item in the case, two items in the case respectively.

- <span style="background-color:yellow">**Initialize the bitmask**</span>: In some cases like the example mentioned above, we need to initialize the bitmask to represent every case is available.

```java
// initialize the bitmask
// base = 3;
// n = 5;
int bitmask = (int)Math.pow(base, n) - 1;

/**
Explanation:
  There are 5 cases, and each case can contain at most 2 items, so base = 3, n = 5.
  Initialize the bitmask to represent each case has two available rooms to hold items now.
  bitmask = (int)Math.pow(base, n) - 1 = 22222(3-based).
**/
```

- <span style="background-color:yellow">**Check the value of the ith bit**</span>

```java
// check the value of the ith bit (counting bitmask and i from right to left, 0-indexed)
// base = 3;
// n = 5;
// bitmask = 22212(3-based)
// i = 1;
int val = bitmask / (int)Math.pow(base, i) % base;

/**
Explanation:
  There are 5 cases, and each case can contain at most 2 items, so base = 3, n = 5.
  bitmask = 22212(3-based), i = 1, so counting from right to left, the ith digit (the 2nd digit) is 1.
**/
```

<span style="background-color:yellow">**Note**:</span>

1. Here we count the bitmask and i both <span style="background-color:yellow">from right to left</span>, so for bitmask 22212 the 1th bit is 1, the least significant bit (LSB) of bitmask is at right-most, so if we convert the bitmask to 10-base number, it should be 2 + 3 + 2 × 9 + 2 × 27 + 2 × 81 = 239.
2. If we count the bitmask and i both <span style="background-color:yellow">from left to right</span>, the formula is still the same, but the value of bitmask and i are different now. So converting bitmask to 10-base number, it should be 2 + 2 × 3 + 2 × 9 + 1 × 27 + 2 × 81 = 215, i should be 3 now (counting from left to right, 0-indexed).
3. In most cases, LSB is at right-most such as in 10-base numbers, so usually we count bitmask from right to left. Then if i is counted from left to right, the above formula should be changed to

```java
// base = 3;
// n = 5;
// bitmask = 22212(3-based) (counting from right to left, become 239 in 10-base)
// i = 3; (counting from left to right, the ith digit has value 1)
int val = bitmask / (int)Math.pow(base, n - 1 - i) % base;
```

- <span style="background-color:yellow">**Decrease the value of the ith bit by 1**</span>

```java
// decrease the value of the ith bit by 1 (counting bitmask and i from right to left, 0-indexed)
// base = 3;
// n = 5;
// bitmask = 22212(3-based)
// i = 2;
bitmask -= (int)Math.pow(bsae, i);

/**
Explanation:
  There are 5 cases, and each case can contain at most 2 items, so base = 3, n = 5.
  bitmask = 22212(3-based), i = 2, so counting from right to left, the ith digit (the 3rd digit) is 2.
  Decrease the third digit by 1, the bitmask should become 22112(3-based) now.
**/
```

<span style="background-color:yellow">**Note**:</span>

1. Same as we discussed in the above section of checking the value of the ith bit, if we count the bitmask from right to left but i from left to right, the formula should be changed to:

```java
// base = 3;
// n = 5;
// bitmask = 22212(3-based) (counting from right to left, become 239 in 10-base)
// i = 2; (counting from left to right, the ith digit has value 2)
bitmask -= (int)Math.pow(bsae, n - 1 - i);
```

### <span style="color:red">Time and Space Complexity</span>

**Time Complexity: <span style="background-color:yellow">O(B<sup>N</sup>)</span>**
If the bitmask has N digits, the base is B, we have at most B<sup>N</sup> different states.

**Space Complexity: <span style="background-color:yellow">O(B<sup>N</sup>)</span>**
For B<sup>N</sup> different states, we need at most B<sup>N</sup> space, in real questions, usually we use much less space.

### <span style="color:red">Further Discussion</span>

Bitmasking can be combined with different algorithms or techniques, here are some examples:

- Bitmask + Dynamic Programming: https://leetcode.com/problems/partition-to-k-equal-sum-subsets/
- Bitmask + BFS: https://leetcode.com/problems/minimum-number-of-flips-to-convert-binary-matrix-to-zero-matrix/
- Non-Binary Bitmask: https://leetcode.com/problems/maximum-and-sum-of-array/

### <span style="color:red">Reference</span>

- https://www.hackerearth.com/practice/algorithms/dynamic-programming/bit-masking/tutorial/
- https://leetcode.com/problems/maximum-and-sum-of-array/discuss/1769139/Python.-Simple-dp-solution-without-bitmask-with-explanation
