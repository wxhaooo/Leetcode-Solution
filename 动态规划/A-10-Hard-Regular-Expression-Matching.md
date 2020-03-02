# 10-Hard: Regular Expression Matching

## 考点

* 动态规划
* 自动机（暂且不看）

### 学到的东西

动态规划如果确定了状态实际上就变成了一个分类讨论问题，对于分类讨论这个技能得掌握一下，对于动态规划乃至所有的算法题而言都很重要。


## 题解

[参考这个题解来理解](https://leetcode-cn.com/problems/regular-expression-matching/solution/dong-tai-gui-hua-zen-yao-cong-0kai-shi-si-kao-da-b/)
```cpp
//动态规划
class Solution {
public:
    bool isMatch(string s, string p) {
        if(p.empty()) return s.empty();
        int m = s.size(),n = p.size();
        std::vector<std::vector<bool>> dp;
        dp.resize(m+1);
        std::for_each(dp.begin(),dp.end(),
        [n](std::vector<bool>& e)
        {
            e.resize(n+1);
            std::fill(e.begin(),e.end(),false);
        });

        dp[0][0] = true;
        for(int i=1;i<=p.size();i++){
            if(p[i-1]=='*') dp[0][i] = true;
            else if(i<p.size() && p[i]=='*') dp[0][i] = true;
            else break;
        }

        for(int i=1;i<m+1;i++){
            for(int j=1;j<n+1;j++){
                if(s[i-1]==p[j-1] || p[j-1]=='.')
                    dp[i][j] = dp[i-1][j-1];
                else if(p[j-1]=='*' && j-2>=0 && (s[i-1]==p[j-2] || p[j-2]=='.')){
                    dp[i][j] = dp[i-1][j] || dp[i][j-1] || dp[i][j-2];
                    //p[j-2]=='.'时(s[i-1]!=p[j-2]，没有continue会继续执行下一个if导致
                    //出错
                    continue;
                }
                //不使用continue时应该在下面的if打开注释部分
                else if(p[j-1]=='*' && j-2>=0 /*&& p[j-2]!='.'*/&& s[i-1]!=p[j-2])
                    dp[i][j] = dp[i][j-2];
            }
        }

        return dp[m][n];
    }
};
```