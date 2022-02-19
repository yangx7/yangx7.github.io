---
title: Depth First Search
date: 2022-02-18
tags:
---

### <span style="color:red">What is Depth First Search?</span>

Depth First Search (DFS) is an <span style="background-color:yellow">**algorithm**</span> usually used to:

- Traverse all nodes in a graph, tree or 2D array.
- Search if there is a path from the root node to the target node in a graph, tree or 2D array.
- Find all paths from the root node to the target node in a graph, tree or 2D array.

---

### <span style="color:red">Template 1</span>

**Recursion template of DFS to find if there is a path from curr node to target node.**

```java
// initially visited should contain curr node
boolean DFS(Node curr, Node target, Set<Node> visited) {
  if (curr == target) {
    return true;
  }

  for (Node next : curr.neighbors) {
    if (!visited.contains(next)) {
      visited.add(next);
      if (DFS(next, target, visited)) {
        return true;
      }
    }
  }

  return false;
}
```
