# 120-Middle: Triangle

## 考点

* 二维动态规划
> 这道题有两个点， __一个点是看出这是个二维动态规划，第二点是要知道怎么找到一个点的邻接的。__ 这两点看出来了。这个题的状态转移方程就非常好写。

## 题解

```cpp
//dp[i][j]=min{dp[i-1][j],dp[i-1][j-1]}+s[i][j]
//照着状态转移方程直接A就行
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        std::vector<std::vector<int>> dp = triangle;
        for(int i=1;i<triangle.size();i++){
            for(int j=0;j<triangle[i].size();j++){
                int tmp = std::numeric_limits<int>::max();
                if(j<triangle[i-1].size()) tmp = std::min(dp[i-1][j],tmp);
                if(j-1>=0) tmp = std::min(dp[i-1][j-1],tmp);
                dp[i][j] = tmp+triangle[i][j];
            }
        }

        return *std::min_element(dp[triangle.size()-1].begin(),dp[triangle.size()-1].end());
    }
};
```

