# 34. Find First and Last Position of Element in Sorted Array
problem [link](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

## "Best" approach
### Intuition
First use binary search to find a target in the array. Then do two binary searches one two sides of the array. On the left side, find the largest element <= target. On the right side, find the smallest element >= target.

### code
```java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int lo = 0;
        int hi = nums.length - 1;
        int[] result = {-1, -1};
        while (lo <= hi) {
            int ptr = (lo + hi) / 2;
            if (nums[ptr] == target) {
                int startIdx = searchStart(nums, lo, ptr - 1, target);
                int endIdx = searchEnd(nums, ptr + 1, hi, target);
                if (nums[startIdx] < target) startIdx++;
                if ( endIdx >= nums.length || nums[endIdx] > target) endIdx--;
                result[0] = startIdx;
                result[1] = endIdx;
                return result;
            } else if (nums[ptr] > target) {
                hi = ptr - 1;
            } else {
                lo = ptr + 1;
            }
        }
        return result;
        
    }
    
    // find the larget value >= target on the interval
    public int searchStart(int[] nums, int start, int end, int target) {
        int lo = start;
        int hi = end;
        while (lo < hi) {
            int ptr = (lo + hi) / 2;
            if (nums[ptr] == target) {
                hi = ptr - 1;
            } else if (nums[ptr] < target) {
                lo = ptr + 1;
            }
        }
        return lo;
    }
    
    // find the smallest value >= target on the interval
    public int searchEnd(int[] nums, int start, int end, int target) {
        int lo = start;
        int hi = end;
        while (lo < hi) {
            int ptr = (lo + hi) / 2;
            if (nums[ptr] == target) {
                lo = ptr + 1;
            } else if (nums[ptr] > target) {
                hi = ptr - 1;
            }
            
        }
        return lo;
    }
}
```

## Comment
This approach is better than directly using two binary searches to find two indexes, as it prevents visiting the same range of values.
