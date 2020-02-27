# 300-Middle: Longest Increasing Subsequence

## 思路

* 动态规划
> 这个动态规划的状态好定义，但是状态转移方程实际不太好想
> 问题仍然出在状态转移方程想不出上
> 重新定义状态可以使得状态成为一个有序数组从而把复杂度降到O(nlogn),非常牛逼
## 题解

```cpp
//dp[i]定义为从s[0]-s[i]且以i结尾的最大上升子序列的长度，leetcode上写的不对
//dp[i] = max(dp[i],dp[j]+1) j ~s[0,i)
//复杂度O(n^2)
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        //dp[i] = max(dp[i-1]+1,d[i-1])
        if(nums.size()==0) return 0;
        std::vector<int> dp(nums.size(),1);
        for(int i=1;i<dp.size();i++){
           for(int j=0;j<i;j++){
               //如果比子串最大的（即s[j]）还大，就可以扩展
               if(nums[i]>nums[j]) 
               dp[i] = std::max(dp[j] +1,dp[i]);
           }
        }
        //返回最大的那个子序列长度
        return *std::max_element(dp.begin(),dp.end());
    }
};
```