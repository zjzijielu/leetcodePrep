# 137. Single Number II
problem [link](https://leetcode.com/problems/single-number-ii/)

## Solution
### Intuition
* We count the number of 1's that appear at each bit. The counter will be reset to 0 whenever it reaches 3.
* The number of counters that we use is based on the number of bits that represent 3. In our case, we need a 2 counters x2, x1 (x1 being the counter for the smallest bit) to represent 2 bits
* Since we are using two counters to represent a 2-bit value, incrementing the counter has become more complicated. It works as follows
    * We use XOR to increment the smallest bit. For example,
    ```java
    0 XOR 0 = 0
    0 XOR 1 = 1
    1 XOR 1 = 0
    ```
    * As we see in the 3rd situation shown above, we need to carry 1 bit to our 2nd smallest bit counter. We first check whether x1 is already 1 before adding the two number. 
    Then we check if the smallest bit of the number that we are adding with is also 1.
    So we use AND to get the carry of addition, then use XOR to to add the carry and x2 together.
    ```java
    carry = x1 & nums[i]; // if 1 there is a carry, vice versa
    x2 = x2 XOR carry;
    ```
* Now we need to reset the counter when it reaches 3. This is solved by applying a mask to every bit counter. The mask is computed as follows
```java
mask = ~(x2 & x1);
x1 &= mask;
x2 &= mask;
```
* Lastly, we return the smallest bit counter. The reason we only return the last bit counter is that
Since there will be 3k + 1 number, the 3k number will gaurantee that the bit counter will be reset to 0. 
The one single number will result in an extra 1 in the smallest bit counter.

### Code
```java
class Solution {
    public int singleNumber(int[] nums) {
        int x1 = 0, x2 = 0, mask = 0;
        for (int i : nums) {
            x2 ^= x1 & i;
            x1 ^= i;
            mask = ~(x1 & x2);
            x1 &= mask;
            x2 &= mask;
        }
        return x1;
    }
}
```

## Comment
* The solution and explanation is provided by fun4LeetCode. I summarized in a way that I understood the solution. 
* The solution can be applied to any generalized problem of "every number appear m times, except for 1 number, which appears only k times (k < m)"
* [TODO] I'm still not so sure how the mask works. 
