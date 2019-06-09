# 130. Surrounded Regions
problem [link](https://leetcode.com/problems/surrounded-regions/)

## Wrong approach
* Go through every element, if `<board[i][j]> == 'O'`, do DFS
* When doing DFS, mark every 'O' as '+'
* If see an 'O' on the boarder, mark previous marked '+' as '-'
* After every element in board, iterate through board again, replace '+' with 'X' and '-' with 'O'
This approach might have worked, but marking previously marked '+' as '-' is too complicated to implement despite its less efficient time and space complexity

## Intuition
* We iterate through the border, and we check if any element if 'O'
* If yes, we mark it as '*', and do DFS
* During DFS, we mark every connected 'O' as '*' 
* We stop DFS if we go out of the board or there is no adjacent 'O' detected
* Then itereate through the board, replace all 'O' with 'X', and '*' with 'O'

## What to notice
* Convert the problem to locate the regions that are connected to the boarder, so we don't have to "back propagate"

## Code 
```java
class Solution {
    public int n;
    public int m;
    public void solve(char[][] board) {
        n = board.length;
        if (n == 0) return;
        m = board[0].length;
        // start from the border
        for (int i = 0; i < n; i++) {
            if (board[i][0] == 'O') {
                if (board[i][0] != '*') {
                    DFSMarking(board, i, 0);
                }
            }
            if (board[i][m-1] == 'O') {
                if (board[i][m-1] != '*') {
                    DFSMarking(board, i, m-1);
                }
            }
        }
        for (int i = 0; i < m; i++) {
            if (board[0][i] == 'O') {
                if (board[0][i] != '*') {
                    DFSMarking(board, 0, i);
                }
            }
            if (board[n-1][i] == 'O') {
                if (board[n-1][i] != '*') {
                    DFSMarking(board, n-1, i);
                }
            }
        }
        // iterate again
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                if (board[i][j] == '*') {
                    board[i][j] = 'O';
                } else if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                }
            }
        }
    }
    
    public void DFSMarking(char[][] board, int i, int j ) {
        if (i < 0 || j < 0 || i >= n || j >= m || board[i][j] == 'X' || board[i][j] == '*') return;
        board[i][j] = '*';
        DFSMarking(board, i+1, j);
        DFSMarking(board, i-1, j);
        DFSMarking(board, i, j+1);
        DFSMarking(board, i, j-1);
    }
}
```
