# Longest Substring with At Least K Repeating Characters
problem [link](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/)

## Solution 1
### Intuition
Firtst, iterate through the string and use a int array `counts` to record the number of occurrences of every letter in the string. Then iterate
through the array `counts` to check if every existing letter has occurred more than `k` times. If so, return the string length.
If not, we first initialize `begin = 0`. We use another pointer `cur` to check if `counts[s.charAt(cur) - 'a'] < k` at every index. 
If so, we calculate the length of the longest substring that statisfies the requirement in `s.substring(start, cur)`.
And we update `start = cur + 1` and `max` accordingly.

We repeat the process for every index.

### Code
```java
class Solution {
    public int longestSubstring(String s, int k) {
        int[] counts = new int[26];
        for (int i = 0; i < s.length(); i++) {
            counts[s.charAt(i) - 'a']++;
        }
        
        boolean valid = true;
        for (int i = 0; i < 26; i++) {
            if (counts[i] > 0 && counts[i] < k) {
                valid = false;
                break;
            }
        }
        if (valid) return s.length();
        
        int result = 0, start = 0, cur = 0;
        while (cur < s.length()) {
            if (counts[s.charAt(cur) - 'a'] < k) {
                result = Math.max(result, longestSubstring(s.substring(start, cur), k));
                start = cur + 1;
            }
            cur++;
        }
        
        result = Math.max(result, longestSubstring(s.substring(start), k));
        return result;
    }
}
```

### Complexity analysis
* O(n) for time complexity. Every element is at most visited twice in addition to counting frequency and checking validity.
* O(n) for space complexity
