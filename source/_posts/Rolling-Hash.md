---
title: Rolling Hash
date: 2022-03-18
tags: algorithm
---

### <span style="color:red">What is Rolling Hash?</span>

- Rolling Hash is a <span style="background-color:yellow">**technique**</span> usually used to match a substring, it is also known as Rabin-Karp Algorithm.
- In rolling hash, we define a hash function to map a string to an unique hash value. Note that hash collision is not evitable, but we can reduce collision by adopting a relative larger modulo or base, we can also compare strings to check if they are really the same after checking the hash value.

### <span style="color:red">Template 1</span>

Implement strStr(), return the index of the first occurrence of string t in string s, or -1 if t is not part of s.
Constraints:

- 0 <= s.length, t.length <= 5 × 10<sup>4</sup>
- s and t consist of only lower-case English characters.

```java
public int strStr(String s, String t) {
  int m = s.length();
  int n = t.length();
  if (m < n) {
    return -1;
  }

  // if t is an empty string, return 0
  if (n == 0) {
    return 0;
  }

  // (int)1e9 + 7 is commonly used, mod can be other value
  int mod = (int)1e9 + 7;
  // since s and t consist of only lower-case English characters, we can use 26 or higher value
  int base = 26;

  // should intialize power with long type,
  // since in the process of calculating power, the value may exceed int range
  // 26 ^ 7 > 2 ^ 31
  long power = 1;
  for (int i = 0; i < n; i++) {
    power = power * base % mod;
  }

  // calculate the hash value of string t,
  // although we will finally mod hash value and it will be in the range of int type,
  // in the process of calculating hash, the value may exceed int range
  long hash = 0;
  for (char c : t.toCharArray()) {
    hash = (hash * base + c - 'a') % mod;
  }

  long h = 0;
  for (int i = 0; i < m; i++) {
    // first add current character to h
    h = (h * base + s.charAt(i) - 'a') % mod;
    // if the length of the substring is less than n, we just continue
    if (i < n - 1) {
      continue;
    }
    // if the length of the substring is larger than n,
    // then decrease the hash value of the first character of the substring
    if (i > n - 1) {
      h = (h - (s.charAt(i - n) - 'a') * power[n] % mod + mod) % mod;
    }
    // return if h == hash and the real substring is the same as t
    if (h == hash && s.substring(i - n + 1, i + 1).equals(t)) {
      return i - n + 1;
    }
    // if we do not worry about collision and want to be faster,
    // we can simply use h == hash as the condition check
  }

  return -1;
}
```

#### Key Attributes:

- The hash function in this template is:
  hash = (t[0] × base<sup>n - 1</sup> + t[1] × base<sup>n - 2</sup> + ... + t[n - 2] × base<sup>1</sup> + t[n - 1] × base<sup>0</sup>) % mod
- (int)1e9 + 7 is commonly used as mod and 26 is usually used as base for strings, but we can make it larger such as 29, etc.
- In this template, we only calculate base<sup>n</sup> as the power, in some questions, we may need to access the power array:

```java
long[] powers = new long[n + 1];
powers[0] = 1;
for (int i = 1; i <= n; i++) {
  powers[i] = powers[i - 1] * base % mod;
}
```

**Time Complexity: <span style="background-color:yellow">O(m + n)</span>**
m is the length of string s, n is the length of string t, we iterate two strings to calculate hash value.

**Space Complexity: <span style="background-color:yellow">O(1)</span>**
We do not use extra space.

### <span style="color:red">Further Discussion</span>

Sometimes the question may define a different hash function, for example:
hash = (t[0] × base<sup>0</sup> + t[1] × base<sup>1</sup> + ... + t[n - 2] × base<sup>n - 2</sup> + t[n - 1] × base<sup>n - 1</sup>) % mod
In this case, we need to calculate hash from the end of the string, this way the middle result won't exceed int and long range.

**related questions:**

- https://leetcode.com/problems/find-substring-with-given-hash-value/

### <span style="color:red">Reference</span>

None
