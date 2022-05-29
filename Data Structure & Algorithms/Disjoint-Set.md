---
tags:
  - disjoint-set
  - union-find
  - data-structure
  - algorithm
---

# Disjoint-Set
A ***Disjoint Set*** is a data structure that stores a collection of disjoint (non-overlapping) sets. This can also be a partition of a set that is stored into disjoint subsets.

> [!NOTE]
> ***Disjoint Set***  also goes by ***Union Find***.

This data structure typically has the following methods:
  * Add Sets
  * Merge/Replace Sets
  * Find parent element (or self if root)

A typical implementation of this structure is called a ***disjoint-set forest***, which performs unions and finds in near constant amortized time.

## Example: Lexigraphically Smallest String (-> [leetcode](https://leetcode.com/problems/smallest-string-with-swaps/))
You are given a string `s` and an array of `s`-index pairs  `pairs`, where `pairs[i] = [a, b]` , where `a` and `b` are indices in `s`.

You can swap the characters at any pair of indices **any number of times**.

*Return the lexigraphically smallest string that `s` can be changed to after using the swaps.*

### Solution (->[post](https://leetcode.com/problems/smallest-string-with-swaps/discuss/387524/Short-Python-Union-find-solution-w-Explanation))
If `[0, 1]` is a pair and `[0, 2]` is a pair, then any pair in `(0, 1, 2)` can be swapped.

This means we can connect (or **union**) different pairs togethers so long as one of their numbers match, and continue doing so as we make larger connections.

After going through all the different pairs, we would have one or more sets that can be used for exchanges.

```python
class UnionFind:
	"""Union-Find/Disjoint-Set Forest
	"""
	def __init__(self, n: int):
		"""Create new Union-Find Forest of size n.
		
		Args:
			n (int): Size of set (expected values to be int-indexed).
		"""
		self.p = list(range(n))
	
	def union(self, x: int, y: int):
		"""Perform union on two indices
			
		Args:
			x (int): First merge index.
			y (int): Second merge index.
		"""
		self.p[self.find(x)] = self.find(y)
	
	def find(self, x: int) -> int:
		"""Returns root of index
		
		Args:
			x (int): index to find root of.
		
		Returns:
			int: Root index
		"""
		if x != self.p[x]:
			self.p[x] = self.find(self.p[x])
		return self.p[x]


def smallest_string_with_swaps(self, s: str, pairs: List[List[int]]) -> str:
	"""Returns the lexigraphically smallest strings with limited swap-pairs.
	
	Args:
		s (str): The string to be modified and returned.
		pairs (List[List[int]]): The list of int-index pairs.
	
	Returns:
		str: Lexigraphically smallest string.
	"""
	
	# Create Union-Find Forest with length of string.
	uf = UnionFind(len(s))
	
	# Create response list for each disjoint set
	res = []
	
	# Create a dict for storing each root key
	m = defaultdict(list)
	
	# Unionize each pair
	for x, y in pairs:
		uf.union(x, y)
	
	# Find root of each index and append character to list.
	for i in range(len(s)):
		m[uf.find(i)].append(s[i])
	
	# Sort each sub-string in each set (reverse because pop removes last elemnent)
	for comp_id in m.keys():
		m[comp_id].sort(reverse=True)
	
	# Pop all characters to list
	for i in range(len(s)):
		res.append(m[uf.find(i)].pop())
	
	# Join all sub-strings to one string and return 
	return ''.join(res)
```
