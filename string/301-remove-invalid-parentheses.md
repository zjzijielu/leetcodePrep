# 301. Remove Invalid Parentheses
problem [link](https://leetcode.com/problems/remove-invalid-parentheses/)

## Solution
### Intuition
When we check if the parentheses are valid, we use a stack to store the `'('` index. When we see a `')'`, we first check if the stack is empty.
If not, we pop the stack. If yes, that means the string until now is invalid, and we need to remove extra `')'`. 

For example, if the string is `'()())()'`, there are three options to remove the redundant `')'`: index 1, 3, 4. However, removing `')'` at
index 3 and 4 are the same. So we need to avoid getting the same string after removal. One way to do so is to use a set. Another way 
is that when we remove a `')'`, we make sure the previous character in the string is not a `')'`.

Now we need to deal with the extra `'('`. What we can do is to reverse the string, and repeat the above process, but instead we checking `'('`.

### Code
```java
class Solution {
    public List<String> removeInvalidParentheses(String s) {
        List<String> ans = new ArrayList<String>();
        remove(s, ans, 0, 0, new char[] {'(', ')'});
        return ans;
    }
    
    public void remove(String s, List<String> ans, int last_i, int last_j, char[] par) {
        for (int stack = 0, i = last_i; i < s.length(); i++) {
            if (s.charAt(i) == par[0]) stack++;
            if (s.charAt(i) == par[1]) stack--;
            if (stack >= 0) continue;
            
            for (int j = last_j; j <= i; j++) {
                if (s.charAt(j) == par[1] && (j == last_j || s.charAt(j - 1) != par[1])) {
                    remove(s.substring(0, j) + s.substring(j + 1, s.length()), ans, i, j, par);
                }
            }
            return;
        }
        
        String reversed = new StringBuilder(s).reverse().toString();
        if (par[0] == '(') {
            remove(reversed, ans, 0, 0, new char[] {')', '('});
        } else {
            ans.add(reversed);
        }
    }
}
```

### Time complexity
* [TODO] time complexity
* [TODO] O(n^2) for space complexity.
