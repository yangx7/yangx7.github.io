---
title: Trie
date: 2022-02-18
tags:
---

### <span style="color:red">What is Trie?</span>

- A Trie is a special form of a Nary tree.
- Typically, Trie is a <span style="background-color:yellow">**data structure**</span> to be used to store and search strings. Each Trie node represents a string (a prefix).
- The root node is associated with the empty string.
- All the descendants of a node have a common prefix of the string associated with that node. That's why Trie is also called prefix tree.
- Trie is widely used in various applications, such as autocomplete, spell checker, etc.

### <span style="color:red">Two representations of a Trie node</span>

- The first solution is to use an **array** to store children nodes.

```java
class TrieNode {
  // change this value to adapt to different cases
  public static final N = 26;
  public TrieNode[] children = new TrieNode[N];

  // you might need some extra values according to different cases
};
```

It is really fast to visit a child node. If we are pursing fast time performence, we can use array to store TrieNode. It is comparatively easy to visit a specific child since we can easily transfer a character to an index in most cases. But not all children nodes are needed. So there might be some waste of space.

- The second solution is to use a **hashmap** to store children nodes.

```java
class TrieNode {
  public Map<Character, TrieNode> children = new HashMap<>();

  // you might need some extra values according to different cases
};
```

It is even easier to visit a specific child directly by the corresponding character. But it might be a little slower than using an array. However, it saves some space since we only store the children nodes we need. It is also more flexible because we are not limited by a fixed length and fixed range.

### <span style="color:red">Implement Trie</span>

Implement a trie with **insert**, **search**, and **startsWith** methods.

```java
class Trie {
  class TrieNode {
    public Map<Character, TrieNode> children = new HashMap<>();
    public boolean isWord;
  }

  private TrieNode root;

  public Trie() {
    root = new TrieNode();
  }

  // Insert a word into the Trie
  public void insert(String word) {
    TrieNode curr = root;
    for (char c : word.toCharArray()) {
      curr.children.putIfAbsent(c, new TrieNode());
      curr = curr.children.get(c);
    }
    curr.isWord = true;
  }

  // Return true if the word is in the Trie
  public boolean search(String word) {
    TrieNode curr = root;
    for (char c : word.toCharArray()) {
      if (curr.children.get(c) == null) {
        return false;
      }
      curr = curr.children.get(c);
    }
    return curr.isWord;
  }

  // Return true if there is any word in the Trie that starts with the given prefix
  public boolean startsWith(String prefix) {
    TrieNode curr = root;
    for (char c : prefix.toCharArray()) {
      if (curr.children.get(c) == null) {
        return false;
      }
      curr = curr.children.get(c);
    }
    return true;
  }
}
```

### <span style="color:red">Time and Space Complexity</span>

|                      | Trie Constructor                                      | insert                                                | search                                                | startsWith                                            |
| -------------------- | ----------------------------------------------------- | ----------------------------------------------------- | ----------------------------------------------------- | ----------------------------------------------------- |
| **Time Complexity:** | <span style="background-color:yellow">**O(1)**</span> | <span style="background-color:yellow">**O(N)**</span> | <span style="background-color:yellow">**O(N)**</span> | <span style="background-color:yellow">**O(N)**</span> |

If the longest length of the word is N, the height of the Trie will be N + 1. Therefore, the time complexity of all insert, search and startsWith methods will be O(N).

**Space Complexity: <span style="background-color:yellow"> O(M \* N \* K)</span>**
If we have M words to insert in total and the length of words is at most N, there will be at most M \* N nodes in the worst case (any two words don't have a common prefix).

Let's assume that there are maximum K different characters (K is equal to 26 in the case of strings only containing lowercase english characters, but might differs in different cases.) So each node will maintain a map or array whose size is at most K.

Therefore, the sapce complexity will be O(M \* N \* K).

In real cases, the space complexity should be much smaller than our estimation, especially when the distribution of words is dense.

### <span style="color:red">Reference</span>

- https://leetcode.com/explore/learn/card/trie/
