# 49. Group Anagrams
problem [link](https://leetcode.com/problems/group-anagrams/)

## Wrong Solution 
### Intuition
We use a 26-bit-long integer to encode the key of a string. We iterate through the character of each word, and use OR operation for all 1 hot vectors that represent the alphabets.
For example
```
/* abc
a -> 001
b -> 010
c -> 100
a | b | c -> 111
*/
```

### Why it's wrong
However, this only works when the characters don't have repeating characaters. Since "boo" is not an anagram "bob", we need to take into account the number of occurrences of each alphabet.

## Solution 1
### Intuition
We use prime numbers to help us form distinctive key for each anagrams set. Due to the characteristics of prime number,
even if one letter appears more than once, multiplying with the corresponding prime number can still keep track of the number of 
occurrences of each letter.

### Code
```java
class Solution {
    int[] primes = {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101};
    
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<Integer, List<String>> map = new HashMap<Integer, List<String>>();
        int n = strs[0].length();
        
        for (int i = 0; i < strs.length; i++) {
            int key = 1;
            for (int j = 0; j < strs[i].length(); j++) {
                key *= primes[strs[i].charAt(j) - 'a'];
            }
            if (map.containsKey(key)) {
                map.get(key).add(strs[i]);
            } else {
                map.put(key, new ArrayList<String>());
                map.get(key).add(strs[i]);
            }
        }
        
        List<List<String>> result = new ArrayList<List<String>>();
        
        for (int key : map.keySet()) {
            List<String> newList = new ArrayList<String>();
            for (int i = 0; i < map.get(key).size(); i++) {
                newList.add(map.get(key).get(i));
            }
            result.add(newList);
        }
        
        return result;
    }
}
```
### Complexity analysis
* O(NK) for time complexity, N being the number of words, K being the maximum length of the words.
* O(Nk) for space complexity

### Possible issues
Since we are doing multiplication, a very long string might cause overflow.

## Solution 2
### Intuition
Just like the sudoku problem, we use string to record the information of each word but in a smarter way. We iterate 
through every word and record the number of occurrences of each letter. And we use "#" (or not) to differentiate all the numbers
when building a string as the key.

### Code
```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        HashMap<String, List<String>> map = new HashMap<String, List<String>>();
        int n = strs.length;
        
        for (int i = 0; i < n; i++) {
            StringBuilder sb = new StringBuilder();
            String s = strs[i];
            int[] count = new int[26];
            for (char c : s.toCharArray()) {
                count[c - 'a'] += 1;
            }
            for (int j = 0; j < 26; j++) {
                sb.append(count[j]);
            }
            String key = sb.toString();
            if (!map.containsKey(key)) {
                map.put(key, new ArrayList<String>() );
            }
            map.get(key).add(s);
        }
        
        List<List<String>> result = new ArrayList<List<String>>();
        for (String key : map.keySet()) {
            List<String> newList = new ArrayList<String>();
            for (int i = 0; i < map.get(key).size(); i++) {
                newList.add(map.get(key).get(i));
            }
            result.add(newList);
        }
        
        return result;
    }
}
```

### Complexity analysis
* O(NK) for time complexity
* O(NK) for space complexity, but uses more memory than 1st approach as every key now is a string that is at least 52 characters long.
