# 63. Unique Paths II
problem [link](https://leetcode.com/problems/unique-paths-ii/)

## Solution
### Intuition
This is a slightly more complicated version of unique paths. The solution at `dp[i][j]]` depeneds on `dp[i-1][j]` and `dp[i][j-1]`
Except that we have to check if `obstacleGrid[i][j] == 1`. If so, `dp[i][j] = 0`.

### Code
```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int n = obstacleGrid.length;
        int m = obstacleGrid[0].length;
        if (m == 0) {
            return 1;
        }
        int[][] dp = new int[n][m];
        if (obstacleGrid[0][0] == 1 || obstacleGrid[n-1][m-1] == 1) {
            return 0;
        } else {
            dp[0][0] = 1;
        }
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (obstacleGrid[i][j] != 1) {
                    if (i != 0) {
                        dp[i][j] += dp[i - 1][j];
                    }   
                    if (j != 0) {
                        dp[i][j] += dp[i][j - 1];
                    }
                } else {
                    dp[i][j] = 0;
                }
            }
        }
        
        return dp[n - 1][m - 1];
    }
}
```

### Complexity analysis
* O(n * m) for time complexity
* O(n * m) for space complexity

## Better Solution
### Intuition
The logic stays the same, except that we can simplify the dp arrays from 2D to 1D. The intuition is that, at a specific position `(i, j)`,
we only need `dp[i-1][j]` and `dp[i][j-1]`. However, because vertically we can only go down, from `grid[i-1][j]` to `grid[i][j]`, there is only one way to do it.
Which is why we can simply represent `dp[i-1][j]` with `dp[j]` to compress the 2D array to 1D.

### Code
```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int n = obstacleGrid.length;
        int m = obstacleGrid[0].length;
        if (m == 0) {
            return 1;
        }
        int[] dp = new int[m];
        if (obstacleGrid[0][0] == 1 || obstacleGrid[n-1][m-1] == 1) {
            return 0;
        } else {
            dp[0] = 1;
        }
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) { 
                if (obstacleGrid[i][j] == 1) {
                    dp[j] = 0;
                } else if (j > 0) {
                    dp[j] += dp[j - 1];
                }
            }
        }
        
        return dp[m - 1];
    }
}
```
### Complexity analysis
* O(n * m) for time complexity
* O(m) for space complexity
