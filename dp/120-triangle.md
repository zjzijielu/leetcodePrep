# 120. Triangle
problem [link](https://leetcode.com/problems/triangle/)

## Recursive solution
### Intuition
Pretty straight forward with the sub-problem relation: 

`dp[layer][i] = min(dp[layer+1][i], dp[layer+1][i+1]) + traingle[len][i]`

### Code
```java
class Solution {
    int[] dp;
    int maxLevel;
    List<List<Integer>> t;
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        t = triangle;
        maxLevel = n;
        dp = new int[n * (1 + n) / 2];
        
        return helper(1, 0);
    }
    
    public int helper(int level, int i) {
        if (level > maxLevel) {
            return 0;
        } else if (level == maxLevel) {
            return t.get(level - 1).get(i);
        }
        int idx = (level - 1) * level / 2 + i;
        if (dp[idx] != 0) {
            return dp[idx];
        }
        int left = helper(level + 1, i);
        int right = helper(level + 1, i + 1);
        dp[idx] = t.get(level - 1).get(i) + Math.min(left, right);
        return dp[idx];
    }
}
```

### Complexity analysis
* O(n^2) for time complexity, as we only traverese the triangle once. n being the number of layers of the triangle.
* O(n^2) for space complexity, as we store the minimum sum starting at every element in the triangle.

## Iterative solution
### Intuition
In this problem, we can apply iterative solution in a bottom up way. The sub-problem relation mentioned above still holds true. 
One thing to notice is that we don't need a dp list to hold minimum starting from every index. We only care about the next layer if we are doing bottom-up.

### Code
```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int layer = triangle.size();
        int[] dp = new int[layer];
        for (int i = 0; i < layer; i++) {
            dp[i] = triangle.get(layer - 1).get(i);
        }
        
        for (int i = layer - 2; i >= 0; i--) {
            for (int j = 0; j <= i; j++) {
                dp[j] = Math.min(dp[j], dp[j + 1]) + triangle.get(i).get(j);
            }
        }
        
        return dp[0];
    }
}
```

### Complexity Analysis 
* O(n^2) for time complexity
* O(n) for space complexity, as we only stores the sum of current row's next row.
