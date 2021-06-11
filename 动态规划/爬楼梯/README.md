## 70. 爬楼梯
https://leetcode-cn.com/problems/climbing-stairs/

``` Java
class Solution {
    public int climbStairs(int n) {
        int p = 0, q = 0, r = 1;
        for(int i = 1; i <= n; i++) {
            p = q;
            q = r;
            r = p+q;
        }
        return r;
    }
}
```

状态转移方程:
f(x) = f(x-1) + f(x-2)
因为一次只能前进1级或者2级台阶，所以第x级的台阶一定由前两级的方案总数相加而来

从第0级爬到第0级我们可以看作只有一种方案，即f(0)=1；从第0级到第1级也只有一种方案，即爬一级，f(1)=1。这两个作为边界条件就可以继续向后推导出第n级的正确结果。
我们可以用「滚动数组思想」把空间复杂度优化成 O(1)

把这道题泛化，如果给定的不只是1，2 而是1，2，5

```Java
class Solution {
public:
    int climbStairs(int n) {
        int DP[n+1];
        memset(DP, 0, sizeof(DP));
        DP[0] = 1;
        int steps[2] = {1,2};
        for (int i = 1; i <= n; i++){
            for (int j = 0; j < 2; j++){ // 这里如果是1，2，5就是j<3了
                int step = steps[j];
                if ( i < step ) continue;// 台阶少于跨越的步数
                DP[i] = DP[i] + DP[i-step];
            }
        }
        return DP[n];

    }
};
```

## 518. 零钱兑换II
https://leetcode-cn.com/problems/coin-change-2/
    dp[x] 表示金额之和等于x的硬币组合数，目标是求 dp[amount]。<br/>
    第一层for循环for(int coin : coins) <br/>
    第二层for循环for(int i = coin; i <= amount; i++) <br/>
    先按照coin遍历，可以保证不会重复使用之前已经遍历过的coin <br/>

`爬楼梯问题和零钱兑换问题的区别:`
        https://leetcode-cn.com/problems/coin-change-2/solution/ling-qian-dui-huan-iihe-pa-lou-ti-wen-ti-dao-di-yo/

这两道题都属于`一维dp`问题：<br/>
爬楼梯属于`排列问题`，221 & 212是两种 <br/>
硬币这个属于`组合问题`，221&212为一种 <br/>
所以说`注意这两道题的for循环嵌套顺序`，排列问题和组合问题的嵌套顺序是不一样的 <br/>

总结，二维dp的组合数问题和排列数问题 都可以交换嵌套的循环，因为子问题不会变化； <br/>
一维的dp组合数问题和排列数问题 不可以交换嵌套的循环，因为会改变子问题； <br/>
一维的dp组合数问题，交换嵌套的循环，子问题会变成排列数问题； 一维的dp排列数问题，交换嵌套的循环，子问题会变成组合数问题；<br/>

## 279. 完全平方数
https://leetcode-cn.com/problems/perfect-squares/

首先看到这道题，会想到昨天那道题 [518. 零钱兑换](https://leetcode-cn.com/problems/coin-change-2/)

那么我这个方法提供给想要加深对于**组合**理解的朋友们，也就是说，我将用昨天那道题的解题思路来做这道题

首先我们来观察这道题与零钱兑换的**相似之处**，
1. 给定了一个目标值，昨天是总金额，今天是总和 - 也就是说，我们仍然可以用一维数组来解决这个问题
2. 可以采用的数组是固定的，昨天是给定了一个数组，有不同面额的硬币可供选择如[1,2,5] 1园，2园，5园三种硬币<br/>今天这道题虽然没有明确给出，但我们仔细观察就会发现，实际可用的数组也是确定的，那就是[1,4,9....]直到小于等于n的平方根的最后一位
3. 所以说，我们可以采取和昨天相同的思路，先按照这个数组遍历，这样就可以防止重复；再按照dp[]顺序遍历。也是两层for循环

那么我们再来看看**不同之处**：
1. 昨天那道题是让求最大组合数，需要累加
2. 今天这道题让求最小方案，那么我们可以想到要使用Math.min()来求解

```Java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int [n+1];
        int[] nums = new int[(int) Math.sqrt(n)]; // nums数组类似于昨天的coins数组
        for(int i = 0; i <= n; i++) {
            dp[i] = i; // 先假设只有1来构成dp数组的每一个数，这也是最坏情况
        }
        for(int i = 0; i < nums.length; i++){
            nums[i] = (i+1)*(i+1); // 构建nums数组
        }
        for(int num: nums){ // 和昨天相同的思路，两层for循环
            for(int i = num; i <= n; i++) {
                dp[i] = Math.min(dp[i],dp[i-num]+1); // 注意这里，状态转移方程采用min方式
            }
        }
        return dp[n];
    }
}
```
感谢观看！

