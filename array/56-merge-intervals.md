# 56. Merge Intervals
problem [link](https://leetcode.com/problems/merge-intervals/)

## Solution
### Intuition
Suppose the starting value and ending value of each interval is sorted. Given two intervals [s1, e1], [s2, e2], 
if s1 <= s2 <= e1, then the two intervals should be merged. Otherwise, namely if s2 > e1, the two intervals should not be merged.

Therefore, we use two arrays to store starting value and ending value of the intervals respectively. 
We use a pointer to keep track of the smallest starting value, and use another pointer i to check if start[i] > end[i-1].

### Complexity analysis
* O(nlogn) for time complexity, as sorting takes O(nlogn) and merging the intervals only takes O(n)
* O(n) for space complexity

### Code
```java
import java.util.*;

class Solution {
    public int[][] merge(int[][] intervals) {
        int n = intervals.length;
        int[] start = new int[n];
        int[] end = new int[n];
        
        if (n == 0) {
            int[][] result = new int[0][0];
            return result;
        }   
        
        for (int i = 0; i < n; i++) {
            start[i] = intervals[i][0];
            end[i] = intervals[i][1];
        }
        
        Arrays.sort(start);
        Arrays.sort(end);
        
        List<Integer> start_list = new ArrayList<Integer>();
        List<Integer> end_list = new ArrayList<Integer>();
        
        int num = 0;
        int j = 0;
        for (int i = 1; i < n; i++) {
            if (start[i] > end[i - 1]) {
                start_list.add(start[j]);
                end_list.add(end[i - 1]);
                j = i;
                num++;
            }
        }
        
        start_list.add(start[j]);
        end_list.add(end[n - 1]);
        num++;
        
        
        int[][] result = new int[num][2];
        
        for (int i = 0; i < num; i++) {
            result[i][0] = start_list.get(i);
            result[i][1] = end_list.get(i);
        }
        
        return result;
    }
}
```

### Comment
The first solution LeetCode provided was to use connected components in a graph. The code was too complicated and the complexity isn't ideal so we omit that.
