# 35.Search Insert Position
problem [link](https://leetcode.com/problems/search-insert-position/)

## Solution
This problem is not hard but my first few submissions were rejected so I feel it's essential to summarize it. 

### Intuition
Use binary search to find the target number. At the end of binary search, check if the number we found is smaller than the target number, if yes, subtrat the index by 1.

### Code
``` java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int n = nums.length;
        if (n == 0) {
            return 0;
        }
        int lo = 0;
        int hi = n - 1;
        while (lo < hi) {
            int ptr = (lo + hi) / 2;
            if (nums[ptr] == target) {
                return ptr;
            } else if (nums[ptr] > target) {
                hi = ptr - 1;
            } else {
                lo = ptr + 1;
            }
        }
        if (nums[lo] < target) {
            return lo + 1;
        }
        return lo;
    }
}
```

### Better code
However it is essentially the same thing as below
```java
    public int searchInsert(int[] A, int target) {
        int low = 0, high = A.length-1;
        while(low<=high){
            int mid = (low+high)/2;
            if(A[mid] == target) return mid;
            else if(A[mid] > target) high = mid-1;
            else low = mid+1;
        }
        return low;
    }
```
