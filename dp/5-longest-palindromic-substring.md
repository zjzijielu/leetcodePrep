# 5. Longest Palindromic Substring
problem [link](https://leetcode.com/problems/longest-palindromic-substring/)

## Solution 1

### Intuition
Given a string s, if we want to check if it's a palindrom, we can see if
* s[0] == s[-1]
* s[1:-1] is a palindrom

The second condition can be checked via dynamic programming. We use a boolean matrix to store if the input string s[i][j] is a palindrom.

### Code
```java 
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        if (n == 0) {
            return "";
        }
        String res = "";
        
        boolean[][] dp = new boolean[n][n];
        
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                dp[i][j] = (s.charAt(i) == s.charAt(j)) && (j - i < 3 || dp[i + 1][j - 1]);
                
                if (dp[i][j] && j - i + 1 > res.length()) {
                    res = s.substring(i, j + 1);
                }
            }
        }
        
        return res;
    }
}
```
### Complexity analysis
* O(n^2) for time complexity, as we are running a nested loop.
* O(n^2) for space complexity, as we use a table to memoize the boolean values.

## Better Solution
### Intuition
Another way to check if a given string s is a palindrom, we start from the center of the string and expand gradually. 
We check whether the first character matches with the last character, if so, we move the pointers outward, until the characters are not the same any more.

### Code 
My first version
```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        if (n == 0) {
            return "";
        }
        
        int start = 0;
        int end = 0;
        int max = 0;
        
        for (int i = 0; i < n; i++) {
            int len1 = expandFromCenter(s, i, i);
            int len2 = expandFromCenter(s, i, i + 1);
            if (len1 > len2 && len1 > max) {
                start = i - (len1 - 1) / 2;
                end = i + (len1 - 1) / 2;
                max = len1;
            } else if (len2 > len1 && len2 > max) {
                start = i - len2 / 2 + 1;
                end = i + len2 / 2;
                max = len2;
            }
        }
        return s.substring(start, end + 1);
    }
    
    public int expandFromCenter(String s, int i, int j) {
        while (i >= 0 && j < s.length() && s.charAt(i) == s.charAt(j)) {
            i--;
            j++;
        }
        return j - i - 1;
    }
}
```
The part of updating the start and end pointers can be done in a simpler way as follows:
```
  int start = 0;
  int end = 0;
  for (int i = 0; i < n; i++) {
      int len1 = expandFromCenter(s, i, i);
      int len2 = expandFromCenter(s, i, i + 1);
      int len = len1 > len2 ? len1 : len2;
      if (len > end - start) {
          start = i - (len - 1) / 2;
          end = i + len / 2;
      }
```

### Complexity Analysis
* O(n^2) for time complexity, since the worst case for each scan is to go through the entire string.
* O(1) for space complexity, since we only need two pointers.
            
