# 64-Middle: Minimum Path Sum

## 考点

* 二维动态规划
> 这道题的动态规划与No.120的方法类似，非常标准的二维动态规划，没有什么难度

## 题解

```cpp
//dp[i][j]=min{dp[i-1][j],d[i][j-1]}+s[i][j]
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[i].size();j++){
                int tmp =std::numeric_limits<int>::max();
                if(i-1>=0) tmp = std::min(tmp,grid[i-1][j]);
                if(j-1>=0) tmp = std::min(tmp,grid[i][j-1]);
                if(tmp!=std::numeric_limits<int>::max())
                grid[i][j]=tmp+grid[i][j];
            }
        }
        return grid.back().back();
    }
};
```