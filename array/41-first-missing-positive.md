# 41. First Missing Positive
problem [link](https://leetcode.com/problems/first-missing-positive/)

## Solution
The problem isn't hard if hashtable is allowed. The tricky part is that the requirement is to use constant space. 

### Intuition
In order to only use constant space, one way is to use in-place swapping. Therefore, we swap the elements in place until they are all in the "right" position. Before swapping, we check
* if the element that we are looking is (1) <= 0, (2) >= nums.length (3) = i + 1. If so, we look at the next element
* if the index to swap to is already occupied with the same element, we don't swap
* we look at the next element

After swapping, we check the resulting array to see if the nums is [1, 2, 3, ...], and return the corresponding answer

### Complexity analysis
* O(n) for time complexity. O(n) for swapping and O(n) for checking
* O(1) for space complexity, as we are doing in place complexity.

### Code
```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return 1;
        }
        
        int i = 0;
        while (i < n) {
            if (nums[i] == i + 1 || nums[i] <= 0 || nums[i] >= n) {
                i++;
            } else if (nums[nums[i] - 1] != nums[i]) {
                swap(nums, i, nums[i] - 1);
            } else {
                i++;
            }
        }
        
        for (i = 0; i < n; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        return i + 1;
    }
    
    public void swap(int[] nums, int idx1, int idx2) {
        int buff = nums[idx1];
        nums[idx1] = nums[idx2];
        nums[idx2] = buff;
    }
}
```
## Comment
* Great example to show how in place swapping can be done in O(N) to reduce space complexity
* Previously, I thought the swapping must be done recursively. The solution shows the way to achieve in place "sorting".
