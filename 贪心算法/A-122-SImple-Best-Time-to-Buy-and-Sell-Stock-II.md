# 122-Simple: Best Time to Buy and Sell Stock II

## 考点

* 贪心算法
> 贪心主要是找到贪的那个点在哪里，找不到贪心就用不了，这个的贪的点还特别难发现，说着是简单题，其实技巧性还是很强的

## 题解
[参考这个题解，简单易懂](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/solution/best-time-to-buy-and-sell-stock-ii-zhuan-hua-fa-ji/)
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
     int ans =0;

    for(int i=1;i<prices.size();i++){
        if(prices[i]>prices[i-1])
            ans+=(prices[i]-prices[i-1]);
    }
     return ans;   
    }
};
```