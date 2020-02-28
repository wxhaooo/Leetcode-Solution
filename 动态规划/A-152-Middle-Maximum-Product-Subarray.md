# 152-Middle: Maximum Product Subarray

## 考点

* 双递推的动态规划

## 题解

```cpp
//简化后的双动态规划
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        if(nums.size()==0) return 0;

        int cur_max = 1,cur_min=1,ans_max = std::numeric_limits<int>::min();
    
        for(int i=0;i<nums.size();i++){
            if(nums[i]<0)
                std::swap(cur_min,cur_max);
            cur_max = std::max(nums[i],cur_max * nums[i]);
            cur_min = std::min(nums[i],cur_min * nums[i]);

            ans_max = std::max(ans_max,cur_max);
        }

        return ans_max;
    }
};
```

```cpp
//错误解法，dp[i]定义为以s[i]结尾的最大连续子序列成绩的最大值，dp[i] = max(dp[i],dp[i]*s[i])。初值为dp[i]=s[i]
//错我的原因为当前为负，后面可能为正，比如序列[-2,3,-4]
//所以需要同时记录当前最小值
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        if(nums.size()==0) return 0;

        std::vector<int> dp(nums);
        for(int i=1;i<dp.size();i++){
            dp[i] = std::max(dp[i],dp[i-1]*nums[i]);
        }

        return *std::max_element(dp.begin(),dp.end());
    }
};
```