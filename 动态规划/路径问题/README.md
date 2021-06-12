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


