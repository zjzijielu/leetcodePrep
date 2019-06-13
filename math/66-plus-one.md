# 66. Plus One
problem [link](https://leetcode.com/problems/plus-one/)

## Solution 
This is an easy question. I just want to use this file to mark a beautiful solution.

### Intuition
* Start from the last digit, if the current digit is smaller than 9, increment it and return.
* If it's equal to 9, make the digit 0, and move on to the step 1.
* There is only one situation where you will need to add an 1 at the beginning of the array and make everything else 0, which is having all digits as 9.

### Code
```java
public int[] plusOne(int[] digits) {
        
    int n = digits.length;
    for(int i=n-1; i>=0; i--) {
        if(digits[i] < 9) {
            digits[i]++;
            return digits;
        }
        
        digits[i] = 0;
    }
    
    int[] newNumber = new int [n+1];
    newNumber[0] = 1;
    
    return newNumber;
}
```

### Comment
My solution omits the fact that there is only one edge case that will require adding an extra 1 at the beginning of the list.
