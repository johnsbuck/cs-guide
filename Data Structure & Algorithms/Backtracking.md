---
tag:
  - backtracking
  - depth-first-search
  - dfs
  - algorithm
---

# Backtracking
**Backtracking** uses recursion (or a stack) to explore a specific space in order to find one or multiple end conditions.

> [!NOTE]
> Backtracking is equivalent to **Depth-First Search (DFS)**.

Backtracking and DFS search deeply, meaning they look at a given branch of nodes or states and travel backwards once they are unable to proceed further.

> [!INFO]- References
> [LeetCode Post](https://leetcode.com/problems/permutations/discuss/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning))

## General Approach
### Pseudocode
The pseudocode example listed in Wikipedia is defined as the following:

<pre>
<b>procedure</b> backtrack(c) <b>is</b>
    <b>if</b> reject(P, c) <b>then</b> return
    <b>if</b> accept(P, c) <b>then</b> output(P, c)
    s ← first(P, c)
    <b>while</b> s ≠ NULL <b>do</b>
        backtrack(s)
        s ← next(P, s)
</pre>

If we compare this with the depth-first search (DFS) pseudocode, we find several similarities in recursive call, state/node searches, and checking if that state/node is available (`reject` and `w.discovered == true`).

<pre>
<b>procedure</b> DFS(G, v) <b>is</b>
    label v as discovered
    <b>for all</b> directed edges from v to w that are <b>in</b> G.adjacentEdges(v) <b>do</b>
        <b>if</b> vertex w is not labeled as discovered <b>then</b>
            recursively call DFS(G, w)
</pre>

The labeling process can be a class variable, a Wrapper class for the search, or some form of list/set that adds explored nodes as they viewed.

### [Examples](https://leetcode.com/problems/permutations/discuss/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning))
#### Subsets
Given an integer array `nums` of **unique** elements, return _all possible subsets (the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(list, new ArrayList<>(), nums, 0);
    return list;
}

private void backtrack(List<List<Integer>> list , List<Integer> tempList, int [] nums, int start){
    list.add(new ArrayList<>(tempList));
    for(int i = start; i < nums.length; i++){
        tempList.add(nums[i]);
        backtrack(list, tempList, nums, i + 1);
        tempList.remove(tempList.size() - 1);
    }
}
```

#### Permutations
Given an array `nums` of distinct integers, return _all the possible permutations_. You can return the answer in **any order**.

```java
public List<List<Integer>> permute(int[] nums) {
   List<List<Integer>> list = new ArrayList<>();
   // Arrays.sort(nums); // not necessary
   backtrack(list, new ArrayList<>(), nums);
   return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums){
   if(tempList.size() == nums.length){
      list.add(new ArrayList<>(tempList));
   } else{
      for(int i = 0; i < nums.length; i++){ 
         if(tempList.contains(nums[i])) continue; // element already exists, skip
         tempList.add(nums[i]);
         backtrack(list, tempList, nums);
         tempList.remove(tempList.size() - 1);
      }
   }
} 
```

> [!NOTE]
> Can improve backtracking by making nums into a list and checking if num is empty to add tempList to full list. Also removes tempList containing number check.

#### Combination Sums
Given an array of **distinct** integers `candidates` and a target integer `target`, return _a list of all **unique combinations** of_ `candidates` _where the chosen numbers sum to_ `target`_._ You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

It is **guaranteed** that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

```java
public List<List<Integer>> combinationSum(int[] nums, int target) {
    List<List<Integer>> list = new ArrayList<>();
    Arrays.sort(nums);
    backtrack(list, new ArrayList<>(), nums, target, 0);
    return list;
}

private void backtrack(List<List<Integer>> list, List<Integer> tempList, int [] nums, int remain, int start){
    if(remain < 0) return;
    else if(remain == 0) list.add(new ArrayList<>(tempList));
    else{ 
        for(int i = start; i < nums.length; i++){
            tempList.add(nums[i]);
            backtrack(list, tempList, nums, remain - nums[i], i); // not i + 1 because we can reuse same elements
            tempList.remove(tempList.size() - 1);
        }
    }
}
```

#### Palindrome Partitioning
Given a string `s`, partition `s` such that every substring of the partition is a **palindrome**. Return all possible palindrome partitioning of `s`.

A **palindrome** string is a string that reads the same backward as forward.

```Java
public List<List<String>> partition(String s) {
   List<List<String>> list = new ArrayList<>();
   backtrack(list, new ArrayList<>(), s, 0);
   return list;
}

public void backtrack(List<List<String>> list, List<String> tempList, String s, int start){
   if(start == s.length())
      list.add(new ArrayList<>(tempList));
   else{
      for(int i = start; i < s.length(); i++){
         if(isPalindrome(s, start, i)){
            tempList.add(s.substring(start, i + 1));
            backtrack(list, tempList, s, i + 1);
            tempList.remove(tempList.size() - 1);
         }
      }
   }
}

public boolean isPalindrome(String s, int low, int high){
   while(low < high)
      if(s.charAt(low++) != s.charAt(high--)) return false;
   return true;
} 
```
