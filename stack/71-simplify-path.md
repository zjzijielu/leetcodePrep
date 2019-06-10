# 71. Simplify Path
problem [link](https://leetcode.com/problems/simplify-path/)

## 1st approach
### Intuition
Use split('/') to get an array of elements. Iterate through the array, push non-empty element into a stack. Do nothing when seeing '.', pop the stack when seeing '..'

### code
```java
import java.util.*;

class Solution {
    public String simplifyPath(String path) {
        Stack st = new Stack();
        String arr[] = path.split("/");
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < arr.length; i++) {
            if (arr[i].length() != 0) {
                if (arr[i].equals("..")) {
                    if (!st.empty()){
                        st.pop();    
                    }
                } else if (!arr[i].equals(".")) {
                    st.push(arr[i]);
                }
            }
        }
        if (st.empty()) {
            return "/";
        }
        while (!st.empty()) {
            sb.insert(0, "/");
            sb.insert(1, st.pop());
        }
        return sb.toString();
    }
}
```
### Comment
* A drawback is that we have to insert at the front when using a stack and a StringBuilder
* I also tried to use a second stack, just to reverse the elements stored in the stack, which results in very little improvements.

## Better approach
### Intuition
The problem with the stack is that we can't read from the front. So instead we use an array, in which case push->add, pop->remove
### Code
```java
class Solution {
    public String simplifyPath(String path) {
        ArrayList<String> list = new ArrayList<>();
        String arr[] = path.split("/");
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < arr.length; i++) {
            if (arr[i].length() != 0) {
                if (arr[i].equals("..")) {
                    int n = list.size();
                    if (n != 0){
                        list.remove(n - 1);
                    }
                } else if (!arr[i].equals(".")) {
                    list.add(arr[i]);
                }
            }
        }
        int n = list.size();
        if (n == 0) {
            return "/";
        }
        int cursor = 0;
        while (cursor < n) {
            sb.append("/");
            sb.append(list.get(cursor));
            cursor++;
        }
        return sb.toString();
    }
}
```
