# 62-Middle: Unique Paths

## 考点

* 二维动态规划

## 题解

```cpp
//dp[i][j]是到位置i,j的路径数
//dp[i][j] = dp[i-1]+dp[j-1]
//dp[i][j] = 1 (i==0||j==0)
class Solution {
public:
    int uniquePaths(int m, int n) {
        if(m==0 || n==0) return 0;
        std::array<std::array<int,101>,101> dp;
        std::memset(&dp,0,sizeof(dp));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(i==0 || j==0) dp[i][j] = 1;
                else dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};
```