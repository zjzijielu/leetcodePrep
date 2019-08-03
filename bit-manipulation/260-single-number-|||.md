# 260. Single Number III
problem [link](https://leetcode.com/problems/single-number-iii/)

## Solution
### Intuition
This problem is actually easier than single number II. If we XOR all numbers, the resulting value will be the XOR result of the two 
single numbers. Here comes the most important observation. 
> For any two different numbers, there will be at least 1 bit that's different,
which will result as an 1 in the XOR value. Therefore, there will be at least one 1 in XOR value of the two single numbers.

With this observation, we can seperate the numbers into two groups. One group contains number has 1 for the bit that is the 
rightmost 1 in the XOR value, and vice versa. Then we do XOR for the two groups, and the resulting number from each group form the answer.

### Code
```java
class Solution {
    public int[] singleNumber(int[] nums) {
        int[] result = new int[2];
        int num = 0;
        for (int i = 0; i < nums.length; i++) {
            num ^= nums[i];
        }
        
        // find the rightmost 1 in num
        int i = 0;
        while (num % 2 == 0) {
            i++;
            num >>>= 1;
        }
        num = 1 << i;
        
        // 2nd pass seperate the numbers into 2 groups
        int num1 = 0, num2 = 0;
        for (int j = 0; j < nums.length; j++) {
            if ((nums[j] & num) == num) {
                num1 ^= nums[j];
            } else {
                num2 ^= nums[j];
            }
        }
        result[0] = num1;
        result[1] = num2;
        
        return result;
    }
}
```

### Compelxity analysis
* O(n) for time complexity
* O(1) for space complexity
