# 54. Spiral Matrix
problem [link](https://leetcode.com/problems/spiral-matrix/)

## Solution 1: layer by layer
### Intuition
We traverse the outest layer, and place the start pointer at `matrix[n][n]` for the (n-1)th round.

## Solution 2: turning around by using direction offset
### Intuition
We use direction offset and a direction value to update the current position. We swap m and n after traversing through a row or a column.

### Code
```java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> result = new ArrayList<Integer>();
        int m = matrix.length;
        if (m == 0) {
            return result;
        }
        int n = matrix[0].length;
        // dont care about 1st row
        for (int i = 0; i < n; i++) {
            result.add(matrix[0][i]);
        }
        int[] pos = {0, n - 1};
        int[][] dirs = {{1, 0}, {0, -1}, {-1, 0}, {0, 1}};
        int d = 0; // direction
        
        int i = (m - 1) * n; // number of rest of the elements
        
        while (i > 0) {
            for (int j = 1; j < m; j++) {
                i--;
                pos[0] += dirs[d][0];
                pos[1] += dirs[d][1];
                result.add(matrix[pos[0]][pos[1]]);
            }
            --m;
            // swap m and n
            int buff = m;
            m = n;
            n = buff;
            d = (d + 1) % 4;
        }
        
        return result;   
    }
}
```
