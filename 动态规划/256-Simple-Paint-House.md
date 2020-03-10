# 256-Simple: Paint House

## 考点

* 动态规划

题目比较简单，就不写详细题解了

## 题解

```cpp
class Solution {
public:
    int minCost(vector<vector<int>>& costs) {
        if(costs.empty()) return 0;

        int upper = std::numeric_limits<int>::max();
        vector<std::vector<int>> dp(costs.size(),vector<int>(3));

        dp[0][0]=costs[0][0];dp[0][1]=costs[0][1];dp[0][2]=costs[0][2];

        for(int i=1;i<dp.size();i++){
            dp[i][0]= std::min(dp[i-1][1],dp[i-1][2])+costs[i][0];
            dp[i][1]= std::min(dp[i-1][0],dp[i-1][2])+costs[i][1];
            dp[i][2]= std::min(dp[i-1][0],dp[i-1][1])+costs[i][2];            
        }
           
        auto last = dp[costs.size()-1];
        return *std::min_element(last.begin(),last.end());
    }
};
```