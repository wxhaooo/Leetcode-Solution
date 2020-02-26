# 121-Simple: Best Time to Buy and Sell Stock

## 考点

* 动态规划
* __差分方法（区间和可以转换成求差的问题，求差问题，也可以转换成区间和的问题。）__
> 这个想法非常叼，可以把求差问题变成求区间和问题，而区间和问题可以使用dp来解决。(dp[i] = max(0,dp[i-1] + s[i]),dp[i]表示以i结尾的最大连续子数组的和)

## 题解

```cpp
//差分法的精简版，但问题是效率居然差不多！
class Solution {
public:
    int maxProfit(vector<int>& prices) {
       if(prices.size()<=1) return 0;
       int prev = 0,profit = 0;
       for(int i=0;i<prices.size()-1;i++){
           int cur = std::max(0,prev + prices[i+1] - prices[i]);
           profit = std::max(profit,cur);
           prev = cur;
       }
       return profit;
    }
};
```

```cpp
//naive的差分方法，代码还有精简的空间
class Solution {
public:
    int maxProfit(vector<int>& prices) {
       if(prices.size()<=1) return 0;
       std::vector<int> diff(prices.size()-1,0);
       for(int i=0;i<diff.size();i++){
           diff[i] = prices[i+1] - prices[i];
       }

       std::vector<int> dp(diff.size(),0);
       dp[0] = std::max(0,diff[0]);
       int profit = dp[0];
       for(int i=1;i<dp.size();i++){
           dp[i] = std::max(0,dp[i-1] + diff[i]);
           profit = std::max(profit,dp[i]);
       }
       return profit;
    }
};
```

```cpp
// d[i] = max(d[i-1],s[i]-min{s[0-i]})
// 前i天的最大收益 = max{前i-1天的最大收益，第i天的价格-前i-1天中的最小价格}
//效率最高
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()<=1) return 0;
       
        int cur_min = prices[0];
        std::vector<int> dp(prices.size(),0);
        for(int i=1;i<prices.size();i++){
            dp[i] = std::max(dp[i-1],prices[i]-cur_min);
            cur_min = std::min(cur_min,prices[i]);
        }
        return dp[prices.size()-1];
    }
};
```