# 96. Unique Binary Search Trees
problem [link](https://leetcode.com/problems/unique-binary-search-trees/)

## Solution

### My Intuition
There has to be a root node, which leaves us with (n-1) nodes. And we divide these (n-1) nodes into two parts. 
Suppose there are m1 unique BST for the left subtree, which has i nodes, and m2 unique BST for the right subtree, which has (n - 1 - i) nodes, then in total there will be m1 * m2 
unique subtree for the specific division of the nodes. This number will be counted again if we reverse the number of nodes.

### Code
```java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        if (n == 0) {
            return 0;
        } else if (n == 1) {
            return 1;
        } else if (n == 2) {
            return 2;
        }
        
        dp[0] = 1;
        dp[1] = 1;
        dp[2] = 2;
        
        return helper(dp, n);
    }
    
    public int helper(int[] dp, int n) {
        for (int i = 0; i <= (n - 1) / 2; i++) {
            int smaller = dp[i] != 0 ? dp[i] : helper(dp, i);
            if (i != n - i - 1) {
                int bigger = dp[n - i - 1] != 0 ? dp[n - i - 1] : helper(dp, n - i - 1); 
                dp[n] += smaller * bigger * 2;
            } else {
                dp[n] += smaller * smaller;
            }
        }
        return dp[n];
    }
}
```

## Iterative solution
### Code
```java
public int numTrees(int n) {
    int [] G = new int[n+1];
    G[0] = G[1] = 1;
    
    for(int i=2; i<=n; ++i) {
    	for(int j=1; j<=i; ++j) {
    		G[i] += G[j-1] * G[i-j];
    	}
    }

    return G[n];
}
```

### Complexity Analysis
* O(n^2) for time complexity
* O(n) for space complexity
