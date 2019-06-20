# 43. Multiply Strings
problem [link](https://leetcode.com/problems/multiply-strings/)

## Solution
### Intuition

First notice that the product of a number with n digits, and a number with m digits will have no more thant (n + m) digits. 
For example, the biggest number a 2-digit number times a 3-digit number is 99 * 999 < 100 * 1000, which will have 5 digits (2 + 3).

When we multiply two numbers, we align the numbers as follows
![image](https://drscdn.500px.org/photo/130178585/m%3D2048/300d71f784f679d5e70fadda8ad7d68f)

(Somehow) the product of num[i] and num[j], which is at most a 2-digits value, is located at index[i + j, i + j + 1] as shown in the picture above.

Therefore, we multiply all possible values and store them in an int array v[n]. 
We add values at the same index together along with the resulting carry from addition at last index. 

Then we go through the whole array and locate the first digit that is not 0, and build the string from there.

### Code
```java
class Solution {
    public String multiply(String num1, String num2) {
        int n1 = num1.length();
        int n2 = num2.length();
        int n = n1 + n2;
        int[] v = new int[n];
        
        for (int i = n1 - 1; i >= 0; i--) {
            for (int j = n2 - 1; j >= 0; j--) {
                int d1 = num1.charAt(i) - '0';
                int d2 = num2.charAt(j) - '0';
                v[i + j] += (d1 * d2) / 10;
                v[i + j + 1] += (d1 * d2) % 10;
            }
        }
        
        int carry = 0;
        for (int i = n - 1; i >= 0; i--) {
            v[i] += carry;
            carry = v[i] / 10;
            v[i] %= 10;
        }
        
        StringBuilder sb = new StringBuilder();
        int i = 0;
        while (i < n && v[i] == 0) {
            i += 1;
        }
        if (i == n) {
            return "0";
        }
        while (i < n) {
            sb.append(v[i]);
            i++;
        }
        
        return sb.toString();
    }
    
}
```
### Complexity Analysis
* O(m * n) for time complexity, which is the number of multiplication in total
* O(m + n) for space complexity, as we are using an array to store all resulting products, as well as a resulting string.

## Comment
This problem is very similar to string number addition. The tricky part is that one needs to notice the rules of multipliation shown in the picture.
