## 62. 不同路径
[参考宫水三叶的总结](https://mp.weixin.qq.com/s?__biz=MzU4NDE3MTEyMA==&mid=2247485037&idx=1&sn=d6d52c48600e655161e84f25d3402514&chksm=fd9cad72caeb2464e1d8adcd991ec178001472a6c6ddc02a1764bc74ea27a97f71fddbce9975&scene=178&cur_album_id=1773144264147812354#rd)
<br/>
注意边界条件，以及状态转移方程

## 63. 不同路径II
[题目链接](https://leetcode-cn.com/problems/unique-paths-ii/)<br/>
多加的一步就是重新考虑边界条件，以及有障碍物时的情况<br/>
[参考宫水三叶的总结](https://mp.weixin.qq.com/s?__biz=MzU4NDE3MTEyMA==&mid=2247485089&idx=1&sn=fd52fd088a5778c9ee101741d458605d&chksm=fd9cadbecaeb24a8f2115139f438fed36cddd96d00d5249d661684faf33b9874e62883720ae6&scene=178&cur_album_id=1773144264147812354#rd)

## 64. 最小路径和
[题目链接](https://leetcode-cn.com/problems/minimum-path-sum/)<br/>
同样，这个格子可能是由上面的或者左边的而来，那么就选择上面或者左边较小的一个+格子本身的分量 = 这个格子的最小路径和<br/>
状态转移方程：**dp[i][j] = Math.min(dp[i-1][j],dp[i][j-1]) + grid[i][j];(一般情况)**<br/>
[参考宫水三叶的总结](https://mp.weixin.qq.com/s?__biz=MzU4NDE3MTEyMA==&mid=2247485106&idx=1&sn=39adbde98707dc02a99e71f58cad5e7c&chksm=fd9cadadcaeb24bb2295d170f3de8dca0ce8e5acadccafbee82139dfe38ce1984435cd7a50ed&scene=178&cur_album_id=1773144264147812354#rd)<br/>
注意这个问题三叶的拓展，如何记录最小路径，以及怎么用一维数组记录信息<br/>
```Java
int[] g = new int[m*n];
int[] path = new int[m+n];
```

## 120. 三角形最小路径和
这道题一开始我没想出来，后来看三叶的题解才想明白<br/>
**重要的是把这个三角形转化为数组，然后弄清楚边界条件：每一行的最左边和最右边是边界条件，要特殊处理**<br/>
```Java
   2
  3 4
 6 5 7
4 1 8 3
```
我们把这个三角形抽象为以下数组:
```Java
2
3 4
6 5 7
4 1 8 3
```
这里开始要有意识地学会**时间与空间优化了**，先是二维数组O(n^2)解法:
```Java
// 二维数组解法
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[][] dp = new int[n][n];
        dp[0][0] = triangle.get(0).get(0);
        for(int i = 1; i < n; i++) {
            for(int j = 0; j <= i; j++) {
                if(j==0) {
                    dp[i][j] = triangle.get(i).get(j) + dp[i-1][j];
                }
                else if(j==i) {
                    dp[i][j] = triangle.get(i).get(j) + dp[i-1][j-1];
                }
                else {
                    dp[i][j] = triangle.get(i).get(j) + Math.min(dp[i-1][j-1],dp[i-1][j]);
                }
            }
        }
        int res = Integer.MAX_VALUE;
        for(int num:dp[n-1]) {
            res = Math.min(res,num);
        }
        return res;
    }
}
```
然后是优化成一维数组的解法: **核心思想：每一行的状态只与上一行有关** - 很厉害的一个写法，***从下往上看***，这样避免了前一次的更新会影响下一次的数据处理
```Java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int level = triangle.size();
        int[] dp = new int[triangle.get(level - 1).size() + 1];

        for (int i = level - 1; i >= 0; --i) {
            List<Integer> cur = triangle.get(i);
            for (int j = 0; j < cur.size(); ++j) 
                dp[j] = cur.get(j) + Math.min(dp[j], dp[j + 1]);
        }
        return dp[0];
    }
}
```
## 931. 下降路径最小和
首先弄清边界条件，第一行和两边<br/>
关于这道题，我想要记录的是官方题解从下向上的做法
```Java
class Solution {
    public int minFallingPathSum(int[][] A) {
        int N = A.length;
        for (int r = N-2; r >= 0; --r) {
            for (int c = 0; c < N; ++c) {
                // best = min(A[r+1][c-1], A[r+1][c], A[r+1][c+1])
                int best = A[r+1][c];
                if (c > 0)
                    best = Math.min(best, A[r+1][c-1]);
                if (c+1 < N)
                    best = Math.min(best, A[r+1][c+1]);
                A[r][c] += best;
            }
        }

        int ans = Integer.MAX_VALUE;
        for (int x: A[0])
            ans = Math.min(ans, x);
        return ans;
    }
}
```
## 1289. 下降路径最小和II -- 第一道困难题
**重点掌握这道题的思想，优化空间，时间**
因为这道题多了一个限制条件，不能选同一列的，但是其他的位置可以随便选，这样我们就需要每次都枚举出不属于同一列的最小值，时间复杂度为O(n^3)<br/>
优化方法是：每次都记录出上一行的最小值和次小值，以及记录他们的index,这是核心优化思想<br/>
事实上 我们可以优化为3个变量<br/>
* first_sum 表示这一行的最小值；
* first_pos 表示这一行最小值对应的 jmin；
* second_sum 表示这一行的次小值。
```Java
class Solution {
    public int minFallingPathSum(int[][] arr) {
        int n = arr.length;
        int first_sum = 0, second_sum = 0, first_pos = -1;
        for(int i = 0; i < n; i++) {
            int fs = Integer.MAX_VALUE, fp = -1, ss = Integer.MAX_VALUE;
            for(int j = 0; j < n; j++) {
                int cur_sum = (first_pos != j ? first_sum : second_sum) + arr[i][j];
                if(cur_sum < fs) {
                    ss = fs;
                    fs = cur_sum;
                    fp = j;
                }
                else if(cur_sum < ss) {
                    ss = cur_sum;
                }
            }
            
            first_sum = fs;
            first_pos = fp;
            second_sum = ss;
        }
        return first_sum;
    }
}
```

