# 151. Reverse Words in a String
problem [link](https://leetcode.com/problems/reverse-words-in-a-string/)

## Imperfect solution
### Intuition
Iterate through the string from the last character to the first character. Use two pointers to keep track of the latest word's starting and ending index. A word is detected when two pointers is not equal (letters are seen before the space), and we see a space.
### code 
```java
class Solution {
    public String reverseWords(String s) {
        // start from the end
        StringBuilder sb = new StringBuilder();
        int n = s.length();
        if (n == 0) return "";
        int end = n-1;
        int start = n-1;
        for (int i = n-1; i >= 0; i--) {
            if (s.charAt(i) == ' ') {
                if (start != end) {
                    sb.append(s.substring(start + 1, end + 1));
                    sb.append(' ');
                    end = i - 1;
                    start = i - 1;
                } else {
                    end--;
                    start--;
                }
            } else {
                start--;
            }
        }
        if (start != end) {
            sb.append(s.substring(start+1, end+1));
        } else {
            int len = sb.length();
            if (len != 0 && sb.charAt(len - 1) == ' ') {
                sb.deleteCharAt(len - 1);
            }
        }
        return sb.toString();
    }
}
```
## "Better" solution
### Intuiton
Trim the string then split the string into array with respect to space. Then join the words in a reverse order.
### code 
```java
class Solution {
    public static String reverseWords(String s) {
        String arr[] = s.trim().split(" ");
        StringBuilder stringBuilder = new StringBuilder();

        for(int i = arr.length-1; i>=0; i--)
        {
            if(!arr[i].isEmpty())
            {
                stringBuilder.append(arr[i]);
                if(i>0) stringBuilder.append(" ");
            }
        }
        return stringBuilder.toString();
    }
}
```
## Comment
* Both are O(N) solution. The "better" solution uses more memory, but is faster.
* [TODO] Not sure why using trim and split is faster... 
