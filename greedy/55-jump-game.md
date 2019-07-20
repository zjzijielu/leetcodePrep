# 55. Jump Game
problem [link](https://leetcode.com/problems/jump-game/)

## Iterative DP solution
### Intuition
Use an boolean array to record if every index can be reached. Starting from the 1st index, mark every reachable index as `true`. Repeat marking for every index.
In the end return `dp[n - 1]`

### Code
```java
class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;
        boolean[] able = new boolean[n];
        able[0] = true;
        for (int i = 0; i < n; i++) {
            if (able[i] == true) {
                if (nums[i] + i >= n - 1) return true;
                for (int j = 1; j <= nums[i]; j++) {
                    if (able[i + j] == false) {
                        able[i + j] = true;
                    }
                }
            } else {
                return false;   
            }
        }
        return able[n - 1];
    }
}
```

### Complexity analysis
* O(n^2) for time complexity, as the worst case is that last index is reachable from every index.
* O(n) for space complexity, as we use a boolean array.

## One pass Solution
### Intuition
We start from the 1st index and obtain the furthest index that we can reach. Then we move to the furthest index and obtain the furthest index and so on.
We return false if at current index, we cannot take any step.

### Code
```java
class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;
        if (n == 1) return true;
        int cur = 0;
        while (cur < n - 1) {
            int step = nums[cur];
            if (step == 0) return false;
            int nextStep = 1, maxStep = 0;
            if (cur + step >= n - 1) return true;
            for (int i = 1; i <= step; i++) {
                if (cur + i + nums[cur + i] > maxStep) {
                    nextStep = cur + i;
                    maxStep = cur + i + nums[cur + i];
                }
            }
            cur = nextStep;
        }
        
        return true;
    }
}
```

### Complexity analysis
* O(n) for time complexity, as we only traverse the list once.
* O(1) for space complexity

## Optimal Solution 1
### Intuition
We keep track of the furthest index reachable by using `max`. Then at every index, we compare if `i + nums[i] > max`, and update accordingly.
If current index `i > max`, then we return false.

### Code
```java
class Solution {
    public boolean canJump(int[] nums) {
        int n = nums.length;
        int max = 0;
        for (int i = 0; i < n; i++) {
            if (i > max) return false;
            max = Math.max(i + nums[i], max);
        }
        
        return true;
    }
}
```
