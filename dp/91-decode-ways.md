# 91. Decode Ways
problem [link](https://leetcode.com/problems/decode-ways/)

## Solution
### Intuition
The idea is very similar to stair climbing. The difference is that there are certain situations where you cannot take 2 stairs at a time, i.e. 
when 2 characters is larger than 27. When we see a 0, we can't even take a single stair up.

### Code
```java
class Solution {
    public int numDecodings(String s) {
        int n = s.length();
        int[] dp = new int[n];
        if (s.charAt(0) == '0') return 0;
        char[] chars = s.toCharArray();
        
        for (int i = 0; i < n; i++) {
            if (i == 0) {
                dp[i] = 1;
            } else if (i == 1) {
                if (chars[i] == '0' && chars[i - 1] - '2' > 0) return 0;
                if ((chars[0] == '1' || (chars[0] == '2' && chars[1] - '6' <= 0)) && chars[i] != '0') {
                    dp[i] = 2;
                } else {
                    dp[i] = 1;
                }
            } else {
                if (chars[i] == '0' && (chars[i - 1] - '2' > 0 || chars[i - 1] == '0')) {
                    return 0;
                }
                if (chars[i] == '0') {
                    dp[i] = dp[i - 2];
                } else if (chars[i - 1] == '1' || (chars[i - 1] == '2' && chars[i] - '6' <= 0)) {
                    dp[i] = dp[i - 2] + dp[i - 1];
                } else {
                    dp[i] = dp[i - 1];
                }
            }
            
        }
        return dp[n - 1];
    }
}
```

### Complexity analysis
* O(n) for time complexity, as we only traverse the string once.
* O(n) for space complexity, but can be easily reduced to O(1) as we only need `dp[i - 2]` and `dp[i - 1]`

## (Better) code
### Intuition
The problem with my code is that it took to long to debug because of so many edge cases (thanks to the notorious '0'). 
The following code runs a bit slower than mine, but is cleaner.

### Code 
```java
class Solution {
    public int numDecodings(String s) {
        int n = s.length();
        int[] dp = new int[n + 1];
        
        dp[0] = 1;
        dp[1] = s.charAt(0) == '0' ? 0 : 1;
        if (dp[1] == '0') return 0;
        for (int i = 2; i <= n; i++) {
            int first = Integer.valueOf(s.substring(i-1, i));
            int second = Integer.valueOf(s.substring(i-2, i));
            if (first >= 1) {
                dp[i] += dp[i-1];
            }
            if (second >= 10 && second <= 26) {
                dp[i] += dp[i-2];
            }
        }
        
        return dp[n];
    }
}
```

### Comment
* It's faster to compare characters instead of converting a string to an integer.
* The better code is more concise as it uses `dp[i] += dp[i-1]` and `dp[i] += dp[i-2]` and compress all the edge cases into two conditions
