# 53. Maximum Subarray
problem [link](https://leetcode.com/problems/maximum-subarray/)

## Greedy Solution
### Intuition
If we compute the sum up until every index, the resulting graph should be very similar to that of buy and sell stock
![pic](https://leetcode.com/media/original_images/122_maxprofit_1.PNG)

The subarray sum can be obtained by getting the difference between two sums. To get the maximum subarray, we are essentially
looking for the biggest residual.

To do that, we keep track of the minimum of the sum and compare if (sum - min) > maxResidual, if so we update maxResidual.
We update the minimum of the sum whenever we see a smaller one.

### Code
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int n = nums.length;
        int sum = 0, min = 0, res = nums[0];   
        for (int i = 0; i < n; i++) {
            sum += nums[i];
            if (sum - min > res) {
                res = sum - min;
            } 
            if (sum < min) {
                min = sum;
            }
        }
        return res;
    }
}
```

### Complexity Analysis
* O(n) for time complexity, as we only traverse the array once
* O(1) for memory complexity, as we only stores sum, min and res.

## DP solution
### Intuition
We need to find the relation among sub-problems. We first use `maxEndinghere` to record the maximum subarray sum that includes 
element `nums[i]`. The DP part is as follows: `maxEndinghere = max(maxEndinghere + nums[i], nums[i])`. The intuition is that, `maxEndinghere`
either includes `maxEndinghere` that includes last element as well as `nums[i]`, or it only contains `nums[i]` (i.e. previous `maxEndinghere < 0`)

We use another variable `maxSofar` to record the maximum subarray sum that we have encountered.

### Code
```java
class Solution {
    public int maxSubArray(int[] nums) {
        int maxSofar = nums[0];
        int maxTillnow = nums[0];
        int n = nums.length;
        for (int i = 1; i < n; i++) {
            maxTillnow = Math.max(maxTillnow + nums[i], nums[i]);
            maxSofar = Math.max(maxTillnow, maxSofar);
        }
        return maxSofar;
    }
}
```
### Complexity Analysis
* O(n) for time complexity, as we only traverse the array once
* O(1) for memory complexity, as we only need memorize two extra values.

## Comment
* The problem first reminded me of the best time to buy and sell stock problem. But I got stuck at the problem "what if global minimum sum occurred after global maximum sum"
It turns out that you only need track the minimum up until a specific index, and keep updating it.
* When implementing the first and second solution, one thing to keep in mind is that we need to set the initial `res`, `maxSofar` and `maxEndinghere` to `nums[0]`.
This is to avoid the situation where the entire array consists of only negative values.
* I also failed a bunch of times trying to solve the problem by 2 pointers. When we see an optimization problem, it's probably best to use greedy or dp.
