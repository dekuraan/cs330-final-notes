# 11: Trees
## 11.1: Introduction to Trees

tree: a connected undirected graph with no simple circuits

### Theorem 1
> An undirected graph is a tree if and only if there is a unique simple path between any two of its vertices.

rooted tree: A rooted tree is a tree in which one vertex has been designated as the root and every edge is directed away from the root.

parent: the parent of v is the unique vertex u such that there
is a directed edge from u to v (the reader should show that such a vertex is unique)

child: When u is the parent of v, v is called a child of u

sibling: vertices with the same parent are called siblings

ancestors: the ancestors of a vertex other than the root are the vertices in the path from the root to this vertex, excluding the vertex itself and including the root (that is, its parent, its parent’s parent, and soon, until the root is reached)

descendants: The descendants of a vertex v are those vertices that have v as an ancestor.

leaf: A vertex of a rooted tree is called a leaf if it has no children.

internal vertices: Vertices that have children are called internal vertices

subtree: If a is a vertex in a tree, the subtree with a as its root is the subgraph of the tree consisting
of a and its descendants and all edges incident to these descendants.

m-ary tree: A rooted tree is called an m-ary tree if every internal vertex has no more than m children. The tree is called a full m-ary tree if every internal vertex has exactly m children. An m-ary tree with m = 2 is called a binary tree.

ordered rooted tree: An ordered rooted tree is a rooted tree where the children of each internal vertex are ordered

### Binary trees
> if an internal vertex has two children, the first child is called the left child and the second child is called the right child. The tree rooted at the left child of a vertex is called the left subtree of this vertex, and the tree rooted at the right child of a vertex is called the right subtree of the vertex.

### Theorem 2
> A tree with n vertices has n − 1 edges.

### Theorem 3
> A full m-ary tree with i internal vertices contains n = mi + 1 vertices.

### Theorem 4
> A full m-ary tree with
1. n vertices has i = (n − 1)/m internal vertices and l = [(m − 1)n + 1]/m leaves,
2. i internal vertices has n = mi + 1 vertices and l = (m − 1)i + 1 leaves,
3. l leaves has n = (ml − 1)/(m − 1) vertices and i = (l − 1)/(m − 1) internal vertices.

level (of a vertex): The level of a vertex v in a rooted tree is the length of the unique path from the root to this vertex.
- level of the root is 0

height: The height of a rooted tree is the maximum of the levels of vertices

balanced: A rooted m-ary tree of height h is balanced if all leaves are at levels h or h − 1.

### Theorem 5
> There are at most $m^h$ leaves in an m-ary tree of height h.

### COROLLARY 1
> If an m-ary tree of height h has l leaves, then $h ≥ ⌈\log m l⌉$. If the m-ary tree is full and balanced, then $h = ⌈logm l⌉$. (We are using the ceiling function here. Recall that ⌈x⌉ is the smallest integer greater than or equal to x.)

## 11.2 Applications of trees
binary search tree: a binary tree in which each child of a vertex is designated as a right or left child, no vertex has more than one right child or left child, and each vertex is labeled with a key, which is one of the items. Furthermore, vertices are assigned keys so that the key of a vertex is both larger than the keys of all vertices in its left subtree and smaller than the keys of all vertices in its right subtree.
```python
procedure insertion(T: binary search tree,
x: item)
v := root of T
# {a vertex not present in T has the value null }
while v ≠ null and label(v) ≠ x
    if x < label(v) then
        if left child of v ≠ null then v := left child of v
        else add new vertex as a left child of v and set v := null
    else
        if right child of v ≠ null then v := right child of v
        else add new vertex as a right child of v and set v := null
if root of T = null then add a vertex v to the tree and label it with x
else if v is null or label(v) ≠ x then label new vertex with x and let v be this new vertex
return v {v = location of x}
```

decision tree: A rooted tree in which each internal vertex corresponds to a decision, with a subtree at these vertices for each possible outcome of the decision

### THEOREM 1
> A sorting algorithm based on binary comparisons requires at least ⌈log2 n!⌉ comparisons.

### COROLLARY 1
> The number of comparisons used by a sorting algorithm to sort n elements based on binary comparisons is Ω(n log n).

### THEOREM 2
> The average number of comparisons used by a sorting algorithm to sort n elements based on binary comparisons is Ω(n log n).

prefix codes: One way to ensure that no bit string corresponds to more than one sequence of letters is to encode letters so that the bit string for a letter never occurs as the first part of the bit string for another letter.

### ALGORITHM 2 Huffman Coding.
```python
procedure Huﬀman(C: symbols ai with frequencies wi , i = 1, … , n)
F := forest of n rooted trees, each consisting of the single vertex ai and assigned weight wi
while F is not a tree
    Replace the rooted trees T and T ′ of least weights from F with w(T) ≥ w(T ′ ) with a tree having a new root that has T as its left subtree and T ′ as its right subtree. Label the new edge to T with 0 and the new edge to T ′ with 1.
    Assign w(T) + w(T ′ ) as the weight of the new tree.
# {the Huﬀman coding for the symbol ai is the concatenation of the labels of the edges in the unique path from the root to the vertex ai }
```
game trees: We model such
games using game trees; the vertices of these trees represent the positions that a game can be in as it progresses; the edges represent legal moves between these positions.

### Definition 1
> The value of a vertex in a game tree is defined recursively as:
1. the value of a leaf is the payoff to the first player when the game terminates in the position represented by this leaf.
2. the value of an internal vertex at an even level is the maximum of the values of its children, and the value of an internal vertex at an odd level is the minimum of the values of its children.

### THEOREM 3
> The value of a vertex of a game tree tells us the payoff to the first player if both players follow the minmax strategy and play starts from the position represented by this vertex.

## 11.3 Tree Traversal

universal address system: `leaf.leaf.leaf.leaf...`
- 0 < 1 < 1.1 < 1.2 < 1.3 < 2 < 3 < 3.1 < 3.1.1 < 3.1.2 < 3.1.2.1 < 3.1.2.2

![Tree Traversals](https://media.geeksforgeeks.org/wp-content/cdn-uploads/Preorder-from-Inorder-and-Postorder-traversals.jpg)

### Preorder traversal
> Let T be an ordered rooted tree with root r. If T consists only of r, then r is the preorder traversal of T. Otherwise, suppose that T1 , T2 , … , Tn are the sub trees at r from left to right in T. The preorder traversal begins by visiting r. It continues by traversing T1 in preorder, then T2 in preorder, and so on, until Tn is traversed in preorder.
```python
procedure preorder(T: ordered rooted tree)
r := root of T
list r
for each child c of r from left to right
    T(c) := subtree with c as its root
    preorder(T(c))
```
traverse prefix form

### Inorder Traversal
> Let T be an ordered rooted tree with root r. If T consists only of r, then r is the inorder traversal of T. Otherwise, suppose that T1 , T2 , … , Tn are the subtrees at r from left to right. The inorder traversal begins by traversing T1 in inorder, then visiting r. It continues by traversing T2 in inorder, then T3 in inorder, … , and finally Tn in inorder.
```python
procedure inorder(T: ordered rooted tree)
r := root of T
if r is a leaf then list r
else
    l := first child of r from left to right
    T(l) := subtree with l as its root
    inorder(T(l))
    list r
    for each child c of r except for l from left to right
        T(c) := subtree with c as its root
        inorder(T(c))
```
traverse infix form

### Postorder traversal
> Let T be an ordered rooted tree with root r. If T consists only of r, then r is the postorder traversal of T. Otherwise, suppose that T1 , T2 , … , Tn are the subtrees at r from left to right. The postorder traversal begins by traversing T1 in postorder, then T2 in postorder, … , then Tn in postorder, and ends by visiting r.
```python
procedure postorder(T: ordered rooted tree)
r := root of T
for each child c of r from left to right
    T(c) := subtree with c as its root
    postorder(T(c))
list r
```
traverse postfix form

## 11.4: Spanning Trees
### Definition 1
> Let G be a simple graph. A spanning tree of G is a subgraph of G that is a tree containing every vertex of G.

### THEOREM 1
> A simple graph is connected if and only if it has a spanning tree.

depth-first search: can be used to build a spanning tree

backtracking: Depth-first search is also called backtracking, because the algorithm returns to vertices previously visited to add paths

tree edges: The edges selected by depth-first search of a graph are called tree edges

back edges: All other edges
of the graph must connect a vertex to an ancestor or descendant of this vertex in the tree. These edges are called back edges

### ALGORITHM 1 Depth-First Search.
```python
procedure DFS(G: connected graph with vertices v1 , v2 , … , vn )
T := tree consisting only of the vertex v1
visit(v1 )

procedure visit(v: vertex of G)
for each vertex w adjacent to v and not yet in T
    add vertex w and edge {v, w} to T
    visit(w)
```

breadth-first search: can be used to build a spanning tree

### ALGORITHM 2 Breadth-First Search.
```python
procedure BFS(G: connected graph with vertices v1 , v2 , … , vn )
T := tree consisting only of vertex v1
L := empty list
put v1 in the list L of unprocessed vertices
while L is not empty
    remove the first vertex, v, from L
    for each neighbor w of v
        if w is not in L and not in T then
            add w to the end of the list L
            add w and edge {v, w} to T
```

## Minimum Spanning Trees

### Definition 1
> A minimum spanning tree in a connected weighted graph is a spanning tree that has the smallest possible sum of weights of its edges.

### ALGORITHM 1 Prim’s Algorithm.
```python
procedure Prim(G: weighted connected undirected graph with n vertices)
T := a minimum-weight edge
for i := 1 to n − 2
    e := an edge of minimum weight incident to a vertex in T and not forming a simple circuit in T if added to T
    T := T with e added
return T # {T is a minimum spanning tree of G}
```

### ALGORITHM 2 Kruskal’s Algorithm.
```python
procedure Kruskal(G: weighted connected undirected graph with n vertices)
T := empty graph
for i := 1 to n − 1
    e := any edge in G with smallest weight that does not form a simple circuit when added to T
    T := T with e added
return T # {T is a minimum spanning tree of G}
```