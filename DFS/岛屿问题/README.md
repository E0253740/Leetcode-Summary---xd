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
但是这样可能会有问题，会混淆海洋和已遍历的陆地，**所以遍历过的最好改成2** <br/>
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
## 695. 岛屿的最大面积
这道题不一样的地方在于dfs返回类型为int而不是void, 那么递归的写法为return 1 + dfs(grid, r - 1, c) + dfs(grid, r + 1, c) + dfs(grid, r, c - 1) + dfs(grid, r, c + 1); <br/>
```Java
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int nr = grid.length;
        int nc = grid[0].length;
        int size = 0;
        for (int r = 0; r < nr; ++r) {
            for (int c = 0; c < nc; ++c) {
                if (grid[r][c] == 1) { // 首先判断当前格子不为'0',才执行dfs
                    int area = dfs(grid, r, c);
                    size = Math.max(size,area);
                }
            }
        }

        return size;
    }
    int dfs(int[][] grid, int r, int c) {
        int nr = grid.length;
        int nc = grid[0].length;
        if (r < 0 || c < 0 || r >= nr || c >= nc || grid[r][c] != 1) {
            return 0;
        }
        grid[r][c] = 2; // 在每次判断完边界条件后，把当前格子设置为2, 这样就可以避免使用visited[][]数组了
        return 1 +dfs(grid, r - 1, c) + dfs(grid, r + 1, c) + dfs(grid, r, c - 1) + dfs(grid, r, c + 1);
    }
}
```
## 463.岛屿的周长
这道题的返回值也是int, 要弄懂这张图:<br/>
![](https://pic.leetcode-cn.com/66d817362c1037ebe7705aacfbc6546e321c2b6a2e4fec96791f47604f546638.jpg)
旁边是边界，海洋都会给周长带来贡献，但是我们要区分它们的种类；<br/>
```Java
class Solution {
    public int islandPerimeter(int[][] grid) {
        int n = grid.length;
        int m = grid[0].length;
        int res = 0;
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if(grid[i][j]==1) {
                    res += dfs(grid,i,j);
                } 
            }
        }
        return res;
    }
    int dfs (int[][] grid, int i, int j) {
        if(!inArea(grid,i,j)) return 1; // 函数因为「坐标 (r, c) 超出网格范围」返回，对应一条黄色的边
        if(grid[i][j]==0) return 1; // 函数因为「当前格子是海洋格子」返回，对应一条蓝色的边
        if(grid[i][j]!=1) return 0; // 函数因为「当前格子是已遍历的陆地格子」返回，和周长没关系
        grid[i][j] = 2;
        return dfs(grid,i-1,j) + dfs(grid,i+1,j) + dfs(grid,i,j-1) + dfs(grid,i,j+1);
    }
    // 判断坐标 (r, c) 是否在网格中
    boolean inArea(int[][] grid, int r, int c) {
        return 0 <= r && r < grid.length 
        && 0 <= c && c < grid[0].length;
    }
}
```
