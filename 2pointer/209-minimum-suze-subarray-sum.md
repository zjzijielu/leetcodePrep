# 209. Minimum Size Subarray Sum
problem [link](https://leetcode.com/problems/minimum-size-subarray-sum/)

## Solution
### Intuition
The brute force solution is to start iterating from every index `i` and find the subarray such that the sum from `i` to `j` is larger or equal to `s`.
The problem with this solution is that we are doing a lot of repetitive summation. 

To avoid the unnecessary summation, we need to have `sum` to record previous computation. Here we use two pointers `left` and `right`.
`left` starts from 0, and find the rightmost index that satisfies the requirement. Now we can move `left` to the next index, because
we have found the minimum size subarray that starts from index `left`. Then `left++` and subtract `nums[left]` from `sum`, and repeat the process.

When `right` goes out of bound, there are two situations: `sum < s` and `sum >= s`. In the first situation, moving `left` will only 
make `sum` grow smaller, so at this moment we know that the solution does not lie in the interval `[left, right]`, so we return the previously
recorded answer.

### Code
```java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int sum = 0;
        int right = 0;
        int ans = Integer.MAX_VALUE;
        for (int i = 0; i < nums.length; i++) {
            while (sum < s && right < nums.length) {
                sum += nums[right];
                right++;
            }
            if (sum < s) return ans == Integer.MAX_VALUE ? 0 : ans;
            ans = Math.min(ans, right - i);
            sum -= nums[i];
        }
        return ans == Integer.MAX_VALUE ? 0 : ans;
    }
}
```
### Complexity analysis
* O(n) for time complexity, as every element will get visited twice by `left` and `right`
* O(1) for space complexity
