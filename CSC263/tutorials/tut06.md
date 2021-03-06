iUTORIAL 6: Disjoint sets
=========================

Recall the disjoint sets data structure
---------------------------------------

Maintains a collection S = {S_1, S_2,...,S_k} of disjoint sets
  -> Each set is identified by a representative (some member of the set)  

Operations:
MAKE-SET(x)
UNION(x, y)
FIND-SET(x)

Note: No duplicate elements are allowed!

Disjoint set application
------------------------

Example: Determine the connected components of an undirected graph.
 
Graph G:
 
a - b   d   f - g
\  /    |   | / |
 c      e   h   i
 
Connected components: {a, b, c}, {d, e}, {f, g, h, i}
(Make sure they understand what a graph and component is.)
 
Goal: Given vertices v_1 and v_2 determine whether they belong to the
same component
 
Question: How do you use disjoint sets to solve this problem?
(Let them guess the answer.)

Connected_Components(G):
  for each vertex v \in V[G]
      do MAKE_SET(v)
  for each edge (u,v) \in E[G]
      do if FIND_SET(u) \neq FIND_SET(v)
         then UNION(u,v)
 
Same_Component(u,v):
  if FIND_SET(u) = FIND_SET(v)
  then return true
  else return false
 
Run this algorithm on the graph above.
 
Edge processed                Collection of disjoint sets
                              {a}, {b}, {c}, {d},...
(a,b)                         {a,b}, {c}, {d},....
(a,c)                         {a,b,c},{d},{e},....
(b,c)                         {a,b,c},{d},{e},....
(d,e)                         {a,b,c}, {d,e}, {f}, {g},....
etc.
(Finish this example if they are interested.)


Recall the linked list implementation
--------------------------------------

               --------------
              |  -----       |
              v /     |      |
head ->       c    -> h  ->  t nil   <- tail

the representative of the set = the first element in the list
other elements may appear in any order in the list

the node contains pointers to:
-the next element
-the representative 

Weighted-union heuristic:
append the smaller list onto the longer, and break ties arbitrarily.

Running time?
-------------
We will analyze the running times of disjoint-set data structures in terms 
of two parameters: 
n = the number of MAKE-SET operations, and 
m = the total number of MAKE-SET, UNION, and FIND-SET operations


Theorem 21.1: Using the linked-list representation of a disjoint set and
the weighted-union heuristic, a sequence of m MAKE_SET, UNION, and
FIND_SET, n of which are MAKE_SET, takes O(m + nlog n) time.
 
MAKE_SET operations:
-each operation takes \Theta(1) each
-so n MAKE_SET operations take time \Theta(n)
-since n >= m, n MAKE_SET operations take time O(m)
 
UNION operations:
-using the weighted-union heuristic, we always append the smaller list
onto the larger one
                                                                                
What is the upper bound on the number of times that an element's
representative must be updated?
 
Consider a fixed element x:
-the first time x's representative is updated:
 x must have started in the smaller set, so the new set has at least two
 members
-the second time x's representative is updated:
 x must have started in the smaller set, so the new set has at least four
 members
-the kth time x's representative is updated, where k \leq n:
 x must have started in the smaller set, so the new set has at least 2^k
 members
 
(Aside: They could prove this using induction.)
 
Therefore, after x's representative pointer has been updated
\lceil log k \rceil times, the new set has at least k members.
 
Since there were n MAKE_SET operations, the largest set can at most
n members
                                                                                
Therefore, x's representative is updated at most \lceil log n \rceil times
 
So the total updated time for all n elements is O(n log n)
 
[Notice that updating the head and tail pointers takes \Theta(1) per
operation, so the total time to update the pointers over at most m
UNION operations is \Theta(m). BUT we note that there can be at most
n UNION operations, so the total time to update pointers is actually
O(n).
 
FIND_SET operations
-------------------
-each FIND_SET operations takes O(1) time
-so at most m FIND_SET operations takes O(m) time
 
Therefore the total time of m MAKE_SET, UNION and FIND_SET operations is:
O(m + n log n)
 
