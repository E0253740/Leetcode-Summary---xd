## 70. 爬楼梯
https://leetcode-cn.com/problems/climbing-stairs/

`// Java
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
}`

状态转移方程:
f(x) = f(x-1) + f(x-2)
因为一次只能前进1级或者2级台阶，所以第x级的台阶一定由前两级的方案总数相加而来

从第0级爬到第0级我们可以看作只有一种方案，即f(0)=1；从第0级到第1级也只有一种方案，即爬一级，f(1)=1。这两个作为边界条件就可以继续向后推导出第n级的正确结果。
我们可以用「滚动数组思想」把空间复杂度优化成 O(1)

把这道题泛化，如果给定的不只是1，2 而是1，2，5

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

## 518. 零钱兑换II
https://leetcode-cn.com/problems/coin-change-2/
    dp[x] 表示金额之和等于x的硬币组合数，目标是求 dp[amount]。
    第一层for循环for(int coin : coins)
    第二层for循环for(int i = coin; i <= amount; i++)
    先按照coin遍历，可以保证不会重复使用之前已经遍历过的coin

`爬楼梯问题和零钱兑换问题的区别:`
https://leetcode-cn.com/problems/coin-change-2/solution/ling-qian-dui-huan-iihe-pa-lou-ti-wen-ti-dao-di-yo/

这两道题都属于`一维dp`问题：
爬楼梯属于`排列问题`，221 & 212是两种
硬币这个属于`组合问题`，221&212为一种
所以说`注意这两道题的for循环嵌套顺序`，排列问题和组合问题的嵌套顺序是不一样的

总结，二维dp的组合数问题和排列数问题 都可以交换嵌套的循环，因为子问题不会变化； 
一维的dp组合数问题和排列数问题 不可以交换嵌套的循环，因为会改变子问题； 
一维的dp组合数问题，交换嵌套的循环，子问题会变成排列数问题； 一维的dp排列数问题，交换嵌套的循环，子问题会变成组合数问题；
