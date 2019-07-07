# 179. Largest Number
problem [link](https://leetcode.com/problems/largest-number/)

## Solution
### Intuition
We first convert all integers to strings. And we sort them in a customized way. When we compare if `string1` is bigger than `string2`
we check if `string1 + string2 > string2 + string1`

For example:
```java
str1 = '3'
str2 = '31'
str1 + str2 = '331'
str2 + str1 = '313'
=> str1 > str2
```

### Code
This is the first problem that involves writing a customized comparator, worth noting.
```java
class Solution {
    public String largestNumber(int[] nums) {
        int n = nums.length;
        String[] str = new String[n];
        for (int i = 0; i < n; i++) {
            str[i] = Integer.toString(nums[i]);
        }
        
        Comparator<String> comp = new Comparator<String>() {
            @Override
            public int compare(String str1, String str2) {
                String result1 = str1 + str2;
                String result2 = str2 + str1;
                return result2.compareTo(result1);
            }
        };
        
        Arrays.sort(str, comp);
        
        StringBuilder sb = new StringBuilder();
        if (str[0].charAt(0) == '0') {
            return "0";
        }
        
        for (int i = 0; i < n; i++) {
            sb.append(str[i]);
        }
        
        return sb.toString();
        
    }
}
```

### Complexity analysis
* O(knlogn) for time complexity. Let's break down complexity for each step
    * O(n) for converting numbers into string
    * Suppose the average length of all strings is k, then compare 2 string will take O(k)
    * Sorting will take O(nlogn)
    * Therefore, in total it takes O(knlogn) + O(n) = O(knlogn) for time complexity
* O(nk) for space complexity
