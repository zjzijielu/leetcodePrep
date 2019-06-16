# 46. Permutations
problem [link](https://leetcode.com/problems/permutations/)

## Solution
This solution to this problem provides a general template to backtracking problems.

### Intuition
Steps of backtracking algorithm
* choose
* backtrack
* unchoose

For a refresher of the algorithm, refer to this [video](https://www.youtube.com/watch?v=78t_yHuGg-0&t=1003s). The only tricky thing here is that the input nums is a int array, which means that you cannot do add or remove inplace. So instead of actually adding or removing, when we do backtracking, we check whether the number has already been chosen, if yes, then skip.

### Code
```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> list = new ArrayList<>();
        backtrack(list, new ArrayList<>(), nums);
        return list;
    }
    
    public void backtrack(List<List<Integer>> list, List<Integer> chosen, int[] nums) {
        if (chosen.size() == nums.length) {
            list.add(new ArrayList<> (chosen));
        } else {
            for (int i = 0; i < nums.length; i++) {
                if (chosen.contains(nums[i])) {
                    continue;
                }
                chosen.add(nums[i]);
                backtrack(list, chosen, nums);
                chosen.remove(chosen.size() - 1);
            }
        }
    }
}
```
