# 198-Simple: House Robber

## 考点

* 动态规划

## 题解

```cpp
//dp[i]表示在[s_0,s_i]之间最大的抢劫数目
//dp[i] = max(dp[i-2]+s[i],dp[i-1]);
//这个dp的点就在于处理不能抢相邻的两个房间
//dp[i-2]+s[i]表示s[i-1]没被抢，可以抢s[i]
//dp[i-1]表示s[i-1]被抢了，不能抢s[i]
//又因为状态转移方程只用到了前两项，所以不需要分配dp数组
//仅用prev1,prev2记录即可
//边界条件dp[0]= s[0](只抢s[0])和dp[-1]=0(一家都抢不到)
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size() == 0) return 0;
        if(nums.size() == 1) return nums[0];

        int prev1 =nums[0],prev2= 0;
        int max_money = 0;
        for(int i=1;i<nums.size();i++)
        {
            int cur = std::max(prev2 + nums[i],prev1);
            max_money = std::max(cur,max_money);
            prev2 = prev1;
            prev1 = cur;
        }
        return prev1;
    }
};
```