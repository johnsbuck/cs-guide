---
tag:
  - breadth-first-search
  - bfs
  - algorithm
---

# Breadth-First Search
**Breadth-First Search (BFS)** searches a given space by looking at each state/node adjacent to the current node, and adding them to a queue. Unlike its counterpart (DFS), BFS searches adjacent nodes first instead of searching a given branch of nodes "deeply".

> [!NOTE]
> Dijkstra's Algorithm a weighted BFS (priority queue).
> A* is a weighted BFS (priority queue).

## General Approach
### Pseudocode
The pseudocode example listed in Wikipedia is defined as the following:

<pre> 1  <b>procedure</b> BFS(<i>G</i>, <i>root</i>) <b>is</b>
 2      let <i>Q</i> be a queue
 3      label <i>root</i> as explored
 4      <i>Q</i>.enqueue(<i>root</i>)
 5      <b>while</b> <i>Q</i> is not empty <b>do</b>
 6          <i>v</i>&nbsp;:= <i>Q</i>.dequeue()
 7          <b>if</b> <i>v</i> is the goal <b>then</b>
 8              <b>return</b> <i>v</i>
 9          <b>for all</b> edges from <i>v</i> to <i>w</i> <b>in</b> <i>G</i>.adjacentEdges(<i>v</i>) <b>do</b>
10              <b>if</b> <i>w</i> is not labeled as explored <b>then</b>
11                  label <i>w</i> as explored
12                  <i>Q</i>.enqueue(<i>w</i>)
</pre>
The labeling process can be a class variable, a Wrapper class for the search, or some form of list/set that adds explored nodes as they viewed.