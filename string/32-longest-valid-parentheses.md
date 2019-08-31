# 32. Longest Valid Parentheses
problem [link](https://leetcode.com/problems/longest-valid-parentheses/)

## Stack approach
### Intuition
When we check if parenthese in a string is valid, we use a stack to push all left parentheses, and when we see a right parentheses,
we pop the stack. If the stack is empty when poping, then the last `')'` doesn't have a valid matching `'('`. But the string until 
the last `')'` contains valid parenthese pairs.

Another observation is that if the stack is not empty by the time we finish, the `'('` left in the stack don't have matching `')'`,
and therefore we only consider the string from `stack.peek()` to `'i'`.

### Code
```java
class Solution {
    public int longestValidParentheses(String s) {
        int max = 0;
        Stack<Integer> stack = new Stack<Integer>();
        stack.push(-1);
        
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else {
                stack.pop();
                if (stack.isEmpty()) {
                    stack.push(i);
                } else {
                    max = Math.max(max, i - stack.peek());
                }
            }
        }
        
        return max;
    }
}
```

## Stack approach w/o using stack
### Intuition
When the parentheses only contain one type of parenthesis, we can check the validity without recording the order of the `'('`s or `')'`.
Therefore we can use two counters `left` and `right` to record the number of each parentheses that we have seen in the string.

We start to scan from left to right first, and whenever we see that `left == right`, we update the max length. When `right > left`, 
we need to reset `left` and `right` to 0 and restart, because it's impossible for the strings to be valid any more. 

We need to do a backward scan as well. Take `'(()'`，we won't be able to detect `'()'`. That is because there is a redudant `'('` at the beginning.

### Code
```java
class Solution {
    public int longestValidParentheses(String s) {
        int max = 0;
        int left = 0;
        int right = 0;
        
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                left++;
            } else {
                right++;
            }
            
            if (left == right) {
                max = Math.max(2 * left, max);
            } else if (right > left) {
                left = 0;
                right = 0;
            }
        }
        
        left = 0;
        right = 0;
        for (int i = s.length() - 1; i >= 0; i--) {
            if (s.charAt(i) == '(')  {
                left++;
            } else {
                right++;
            }
            
            if (left == right) {
                max = Math.max(2 * left, max);
            } else if (left > right) {
                left = 0;
                right = 0;
            }
        }
        
        return max;
    }
}
```

### Complexity anlaysis
* O(n) for time complexity
* O(1) for space complexity

## [TODO] DP approach
