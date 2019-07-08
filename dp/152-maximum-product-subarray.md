# 152. Maximum Product Subarray
problem [link](https://leetcode.com/problems/maximum-product-subarray/)

## Solution 1
### Intuition
The problem is very similar to that of [maximum subarray](https://github.com/zjzijielu/leetcodePrep/blob/master/dp/53-maximum-subarray.md) problem. The difference is that we may encounter 0 and negative numbers, and that's trickier.

The dp relation in maximum subarray problem is that we use a dp array to store the maximums result for every element. `dp[i]` denotes
the maximum result one can get given that `nums[i]` is included in the subarray.

The logic also be applied here, except that since we may have negative values, we also need to keep track of the minimum negative value 
at `dp_min[i - 1]` in case that `dp_min[i - 1] * nums[i] > max`. 

### Code
```java
class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;
        int[] dp_max = new int[n];
        int[] dp_min = new int[n];
        dp_max[0] = nums[0];
        dp_min[0] = nums[0];
        
        int max = nums[0];
        
        for (int i = 1; i < n; i++) {
            if (nums[i] > 0) {
                dp_max[i] = dp_max[i - 1] * nums[i] > nums[i] ? dp_max[i - 1] * nums[i] : nums[i];
                max = dp_max[i] > max ? dp_max[i] : max;
                
                dp_min[i] = dp_min[i - 1] * nums[i] < nums[i] ? dp_min[i - 1] * nums[i] : nums[i];
            } else if (nums[i] == 0) {
                dp_max[i] = 0;
                dp_min[i] = 0;
            } else {
                // nums[i] < 0
                dp_max[i] = dp_min[i - 1] * nums[i] > nums[i] ? dp_min[i - 1] * nums[i] : nums[i];
                max = dp_max[i] > max ? dp_max[i] : max;
                
                dp_min[i] = dp_max[i - 1] * nums[i] < nums[i] ? dp_max[i - 1] * nums[i] : nums[i];
            }
            
        }
        
        return max;
    }
}
```

### Complexity analysis
* O(n) for time complexity, as we only traverse the array once.
* O(n) for space complexity, as we store two values for each element.

## Constant space solution
### Intuition
In fact, we don't really need to store `dp_max` and `dp_min` for each element, as we don't care about `dp_max[0:i-2]` or `dp_min[0:i-2]` 
when we determine `dp_max[i]` and `dp_min[i]` as they only depend on `dp_max[i - 1]` and `dp_min[i - 1]`. 

### Code
```java
class Solution {
    public int maxProduct(int[] nums) {
        int n = nums.length;
        int dp_max = nums[0];
        int dp_min = nums[0];
        
        int max = nums[0];
        
        for (int i = 1; i < n; i++) {
            if (nums[i] > 0) {
                dp_max = Math.max(dp_max * nums[i], nums[i]);
                max = Math.max(dp_max, max);
                dp_min = Math.min(dp_min * nums[i], nums[i]);
            } else if (nums[i] == 0) {
                dp_max = 0;
                dp_min = 0;
            } else {
                // nums[i] < 0
                int max_buff = dp_max;
                dp_max = Math.max(dp_min * nums[i], nums[i]);
                max = Math.max(dp_max, max);
                dp_min = Math.min(max_buff * nums[i], nums[i]);
            }
            
        }
        
        return max;
    }
}
```
### Complexity analysis
* O(n) for time complexity
* O(1) for space complexity
