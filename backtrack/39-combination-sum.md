# 39. Combination Sum
problem [link](https://leetcode.com/problems/combination-sum/)

## Solution 1
### Intuition 
The solution is based on the backtrack framework: add -> explore -> remove. The difference is that I run a for loop to add a 
specific element, say `candidates[i]` until I cannot add this element anymore. Then I go on to explore `candidates[i+1 : n-1]`.
Before exploring, I record the size of array `chosen` with `oldSize`. After exploring, I remove all the elements that are beyond
`oldSize` in `chosen`.

### Code
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        
        for (int i = 0; i < candidates.length; i++) {
            // System.out.printf("now starting with %d\n", candidates[i]);
            helper(result, new ArrayList<>(), candidates, i, target);    
        }
       
        
        return result;
    }
    
    public void helper(List<List<Integer>> l, List<Integer> chosen, int[] candidates, int start, int target) {
        if (target == 0) {
            l.add(new ArrayList<> (chosen));
            return;
        } else if (start == candidates.length) {
            return;
        }
        
        for (int i = 1; i <= target / candidates[start]; i++) {
            chosen.add(candidates[start]);
            int oldSize = chosen.size();
            if (target - i * candidates[start] == 0) {
                helper(l, chosen, candidates, i, 0);
                int newSize = chosen.size();
                for (int k = 0; k < newSize - oldSize; k++) {
                    chosen.remove(chosen.size() - 1);
                }
            } else if (start == candidates.length - 1) {
                helper(l, chosen, candidates, start + 1, target - i * candidates[start]);
                int newSize = chosen.size();
                for (int k = 0; k < newSize - oldSize; k++) {
                    chosen.remove(chosen.size() - 1);
                }
            } else {
                for (int j = start + 1; j < candidates.length; j++) {
                    helper(l, chosen, candidates, j, target - i * candidates[start]);     
                    int newSize = chosen.size();
                    for (int k = 0; k < newSize - oldSize; k++) {
                        chosen.remove(chosen.size() - 1);
                    }
                }
            }
        }
        
    }
}
```
### Comment
One of the advantage of this solution is that you don't need to sort the array. You can iterate through `candidates`.

However, the logic is a bit verbose and can be further improved.

## Better Solution
### Intuition
This solution strictly follows the (add -> explore -> remove) structure, where you add one element to `chosen` and then you explore. After exploring,
remove the element that you just added.

### Code
```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        Arrays.sort(candidates);
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        helper(result, new ArrayList<>(), 0, target, candidates);
        
        return result;
    }
    
    public void helper(List<List<Integer>> l, List<Integer> chosen, int start, int target, int[] candidates) {
        if (target == 0) {
            l.add(new ArrayList<> (chosen));
        } else if (target > 0) {
            for (int i = start; i < candidates.length && target >= candidates[i]; i++) {
                chosen.add(candidates[i]);
                helper(l, chosen, i, target - candidates[i], candidates);
                chosen.remove(chosen.size() - 1);
            }
        }
    }
}
```

### Comment
The logic is much clearer. You need to sort the array first which is to prevent unnecessary iteration later on.
