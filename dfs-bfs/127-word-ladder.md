# 127. Word Ladder
problem [link](https://leetcode.com/problems/word-ladder/)

## Solution 1
### Intuition
* First, think of the word list as a graph where each word is a node, and there is a link between every two words that is only one character different from each other. ![alt text](https://leetcode.com/problems/word-ladder/Figures/127/Word_Ladder_1.png)
* Then the problem has become "finding the shortest path from node A to node B in the graph", which can be solved by BFS
* To find adjacent nodes, we use a hashtable. For example, the word "hot" is adjacent to any word that shares the pattern "*ot", "h*t", "ho*". Therefore, in the hashtable we store the (key, values) pair with key "*ot" and values with a string list of all words that has the pattern of the key.
* We use a queue to store the nodes to visit for BFS. Each element of the queue stores a pair of <word, level>
* Since we are using BFS, the first time we meet the node with the endWord, the level computed is the length of the shorted path. 
* We also keep keep a hashtable to record the nodes that we have visited to prevent cycles

### Cdoe 
```java
import java.util.*;
import javafx.util.Pair;

class Solution {
    public int ladderLength(String beginWord, String endWord, List<String> wordList) {
        HashMap<String, ArrayList<String>> map = new HashMap<String, ArrayList<String>>();
        int m = beginWord.length();
        for (int i = 0; i < wordList.size(); i++) {
            for (int j = 0; j < m; j++) {
                StringBuilder sb = new StringBuilder();
                sb.append(wordList.get(i).substring(0, j));
                sb.append("*");
                sb.append(wordList.get(i).substring(j + 1));
                String key = sb.toString();
                ArrayList<String> values = map.getOrDefault(key, new ArrayList<String>());
                values.add(wordList.get(i));
                map.put(key, values);
            }
        }
        
        // Queue for BFS
        Queue<Pair<String, Integer>> Q = new LinkedList<Pair<String, Integer>>();
        Q.add(new Pair(beginWord, 1));
        
        // Use a new hahs map to store the visited nodes to prevent cycles
        HashMap<String, Boolean> visited = new HashMap<String, Boolean>();
        visited.put(beginWord, true);
        
        while (!Q.isEmpty()) {
            Pair<String, Integer> node = Q.remove();
            String word = node.getKey();
            int level = node.getValue();
            for (int i = 0; i < m; i++) {
                // next pattern to search for
                String key = word.substring(0, i) + "*" + word.substring(i + 1);
                // get the possible words from hashmap
                for (String adjacentWord : map.getOrDefault(key, new ArrayList<String>())) {
                    // if we find the end word, we can return the answer
                    if (adjacentWord.equals(endWord)) {
                        return level + 1;
                    }
                    // Otherwise add to the BFS Queue. Also mark it visited
                    if (!visited.containsKey(adjacentWord)) {
                        Q.add(new Pair(adjacentWord, level + 1));
                        visited.put(adjacentWord, true);
                    }
                }    
            }
            
            
        }
        
        return 0;
    }
  }
```
### Complexity analysis
* O(N * M) for time complexity, where N is the number of elements in wordList and M is the length of each word. In the worst situation, we will traverse through the entire graph and either get a result or not. Therefore, each node will be visted at most once.
* O(N * M) for space complexity, since one word will have M patterns to store, and in the worst situation all patterns are distinct.

### Comments
* It is important to notice that it is essentially a shortest path problem
* Use of pair and queue in java.

## Better Solution
### Intuition
The previous solution can be improved by bidirectional BFS. You can refer to this [page](https://stackoverflow.com/questions/10995699/how-do-you-use-a-bidirectional-bfs-to-find-the-shortest-path) to refresh the idea of bidirectional BFS.

### Code
[TODO] should try it next time by myself then post.
