# 76. Minimum Window Substring
problem [link](https://leetcode.com/problems/minimum-window-substring/)

## Solution
### Intuition
The process can be broken down into the following 3 steps
1. locate the right pointer.
2. subtract the left pointer one at a time.
3. if the `s[left, right]` doesn't contain all required characters, repeat step 1.

### Code
```java
class Solution {    
    public String minWindow(String s, String t) {
        int[] map = new int[128];
        for (int i = 0; i < t.length(); i++) {
            map[t.charAt(i)]++;
        }
        int count = t.length();
        int end = 0, begin = 0, min = Integer.MAX_VALUE, head = 0;
        
        while (end < s.length()) {
            // expand the window
            if (map[s.charAt(end)] > 0) {
                count--;
            }
            map[s.charAt(end)]--;
            end++;
            
            
            // subtract the window
            while (count == 0) {
                if (end - begin < min) {
                    min = end - begin;
                    head = begin;
                }
                
                if (map[s.charAt(begin)] == 0) {
                    count++;
                }
                map[s.charAt(begin)]++;
                begin++;
                
            }
        }
        
        return min == Integer.MAX_VALUE ? "" : s.substring(head, head + min);
    }
}
```

### Complexity analysis
* O(n) for time complexity, as every character will be visited at most once
* O(1) for space complexity, as we always store an int array `map` that is of size 128.

### Comment
* I was having a hard time debugging because I was trying to locate `left` index first. However, you don't have to worry about `left`
pointer until you start to subtract the window.
* When dealing with character, using an int array is (almost) always faster than using a hashmap, even though it might counter intuitive
that hashmap will use less space.
* For a generic template tackling substring problem, refer to [here](https://leetcode.com/problems/minimum-window-substring/discuss/26808/Here-is-a-10-line-template-that-can-solve-most-'substring'-problems)
