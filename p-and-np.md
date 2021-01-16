# P and NP

## P

P is a complexity class that represents the set of all decision problems that can be solved in polynomial time.

That is, given an instance of the problem, the answer yes or no can be decided in polynomial time.

**Example**

Given a connected graph G, can its vertices be coloured using two colours so that no edge is monochromatic?

Algorithm: start with an arbitrary vertex, color it red and all of its neighbours blue and continue. Stop when you run out of vertices or you are forced to make an edge have both of its endpoints be the same color.

## NP

NP (nondeterministic polynomial time) is a complexity class used to classify decision problems

TODO

## NP Complete

## NP Hard

## What should we do if we know a problem is NP-hard/NP-complete?

We have to do searching, and think of ways to optimize the searching performance using tricks e.g. State Compression, Branch Pruning, Heuristic.

## References

* https://stackoverflow.com/questions/1857244/what-are-the-differences-between-np-np-complete-and-np-hard
* https://en.wikipedia.org/wiki/NP_(complexity)
* https://en.wikipedia.org/wiki/NP-completeness