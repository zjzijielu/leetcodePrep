# 136. Single Number
problem [link](https://leetcode.com/problems/single-number/)
## Intuition
* a XOR a = 0
* a XOR 0 = a

Therefore, if we use XOR with all numbers, then the single number will be the result.

## Code 
```java
class Solution {
    public int singleNumber(int[] nums) {
        int result = 0;
        for (int i = 0; i < nums.length; i++) {
            result = result ^ nums[i];
        }
        return result;
    }
}
```

## Comment
The problem can be easily solved by hash table, but it requires extra space. When the a list of number related prolbem specifies no extra space allowed, might want to think about bit manipulation.
