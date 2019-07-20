# 80. Remove Duplicates from Sorted Array II
problem [link](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)

## Solution
### Intuition
The most important here is to realize that in an array `result` that satisfies the requirement that "duplicates appeared at most twice",
the following statement is true for every `i`: `result[i+2] > result[i]`. Therefore, we keep a pointer `i` to represent the tail of the sorted part, and check if an element is larger than `nums[i-2]`

### Code
```java
class Solution {
    public int removeDuplicates(int[] nums) {
        int i = 0;
        for (int n : nums) {
            if (i < 2 || nums[i - 2] < n) {
                nums[i++] = n;
            }
        }
        return i;
    }
}
```

### Complexity analysis
* O(n) for time complexity
* O(1) for space complexity
