# 岛屿问题
**先阅读一下，岛屿问题详解最好的是这个文章：**[岛屿问题的通用解法](https://leetcode-cn.com/problems/number-of-islands/solution/dao-yu-lei-wen-ti-de-tong-yong-jie-fa-dfs-bian-li-/)
## 200. 岛屿数量
这道题是岛屿系列的第一题，也是最经典的一道题，其他的题都是这道题的变形题，一定要把这道题的dfs, visited[][],以及边界条件的思想掌握<br/>
我的做法
```Java
class Solution {
    public boolean visited[][];
    public int n;
    public int m;
    public boolean isIsland;

    public int numIslands(char[][] grid) {
        n = grid.length;
        m = grid[0].length;
        int res = 0;
        visited = new boolean[n][m];
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                isIsland = false;
                dfs(grid, i, j);
                if(isIsland) res++;
            }
        }
        return res;
    }
    public void dfs (char[][]grid, int i, int j) {
        if(i < 0 || i ==n || j <0 || j == m) return; // 记住这些边界条件
        if(visited[i][j]==true) return; // 记住这些边界条件
        if(grid[i][j]=='0') return; // 记住这些边界条件
        isIsland = true; //如果到了这里都没返回，说明当前格子是有效的，我们进行下一步处理
        visited[i][j] = true;
        dfs(grid,i-1,j); // 这四个dfs是递归思想的核心，上，下，左，右
        dfs(grid,i+1,j);
        dfs(grid,i,j-1);
        dfs(grid,i,j+1);
    }
}
```
可以看到，我这里使用了全局变量以及visited[][]数组来防止重复计数，但这样就会比较慢 <br/>
下面是官方题解，官方题解在每次判断完边界条件后，把当前格子设置为'0', 这样就可以避免使用visited[][]数组了.同样，官方题解没有使用全局变量，每次dfs重新得出数组长度 <br/>
```Java
class Solution {
    void dfs(char[][] grid, int r, int c) {
        int nr = grid.length;
        int nc = grid[0].length;

        if (r < 0 || c < 0 || r >= nr || c >= nc || grid[r][c] == '0') {
            return;
        }

        grid[r][c] = '0'; // 在每次判断完边界条件后，把当前格子设置为'0', 这样就可以避免使用visited[][]数组了
        dfs(grid, r - 1, c);
        dfs(grid, r + 1, c);
        dfs(grid, r, c - 1);
        dfs(grid, r, c + 1);
    }

    public int numIslands(char[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }

        int nr = grid.length;
        int nc = grid[0].length;
        int num_islands = 0;
        for (int r = 0; r < nr; ++r) {
            for (int c = 0; c < nc; ++c) {
                if (grid[r][c] == '1') { // 首先判断当前格子不为'0',才执行dfs
                    ++num_islands;
                    dfs(grid, r, c);
                }
            }
        }

        return num_islands;
    }
}
```
