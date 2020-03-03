# 903-Hard: Valid Permutations for DI Sequence

## 考点

* 动态规划
> 这个动态规划超难想，也不知道能从里面学点啥，就先把这道题的解法记住把

## 题解

[题解参看这个题解的法一](https://www.cnblogs.com/grandyang/p/11094525.html)

```cpp
//动态规划的解法
class Solution {
public:
    int numPermsDISequence(string S) {
        //注意这里是1e9而不是10e9，别搞错了
        constexpr int MOD = 1e9 + 7;
        int n = S.size();
        std::vector<std::vector<int>> dp(n+1,std::vector<int>(n+1));
        dp[0][0] = 1;

        for(int i=1;i<n+1;i++){
            for(int j=0;j<=i;j++){
                if(S[i-1]=='D'){
                    for(int k=j;k<=i-1;k++)
                     dp[i][j] = (dp[i][j] + dp[i - 1][k]) % MOD;
                }
                else{
                      for(int k=0;k<=j-1;k++)
                       dp[i][j] = (dp[i][j] + dp[i - 1][k]) % MOD;
                }
                //注释的解法也行，代码好看点，速度慢一些
                // for(int k=0;k<=i-1;k++){
                //     if(S[i-1]=='D' && k>=j && k<=i-1)
                //         dp[i][j] = (dp[i][j] + dp[i-1][k]) % MOD;
                //     else if(S[i-1]=='I' && k>=0 && k<=j-1)
                //         dp[i][j] = (dp[i][j] + dp[i-1][k]) % MOD;
                // }
            }
        }

        int ans = 0;
        for(int j=0;j<=n;j++){
            ans = (ans + dp[n][j]) % MOD;
        }
        return ans;
    }
};
```