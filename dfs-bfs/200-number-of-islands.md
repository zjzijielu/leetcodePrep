# 200. number of islands
problem [link](https://leetcode.com/problems/number-of-islands/)
### Intuition
Go through all the grids, if we see '1' at point (i, j), we do a DFS starting from that point, and we mark out all the 1s. By turning all 1s into 0s, we essentially solve this question in O(n*m)
### code 
```java
class Solution {
    public int n;
    public int m;
    
    public int numIslands(char[][] grid) {
        int count = 0;
        n = grid.length;
        if (n == 0) return 0;
        m = grid[0].length;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (grid[i][j] == '1') {
                    DFSMarking(grid, i, j);
                    count++;   
                }
            }
        }
        return count;
    }
    
    public void DFSMarking(char[][]grid, int i, int j) {
        if (i < 0 || j < 0 || i >= n || j >= m || grid[i][j] == '0') return;
        grid[i][j] = '0';
        DFSMarking(grid, i + 1, j);
        DFSMarking(grid, i - 1, j);
        DFSMarking(grid, i, j + 1);
        DFSMarking(grid, i, j - 1);
    }
}
```
