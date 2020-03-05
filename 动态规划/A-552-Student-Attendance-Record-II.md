# 552-Middle: Student Attendance Record II

## 考点

* 动态规划
> 这几个动态规划连状态都想不来....，官方还有一种动态规划的方式，可以参考看看

## 题解

[参考这个题解](https://leetcode-cn.com/problems/student-attendance-record-ii/solution/dong-tai-gui-hua-jian-dan-yi-dong-by-xin-qing-za-h/)
```cpp
//速度和内存占比都高的惊人，时间1516ms，空间385.9MB
class Solution {
public:
    int checkRecord(int n) {

        if(n==0) return 0;
        if(n==1) return 3;
        if(n==2) return 8;

        constexpr int mod = 1e9+7;

        std::vector<std::vector<std::vector<int>>> dp(n+1,
        std::vector<std::vector<int>>(2,std::vector(3,0)));

        dp[2][0][0]=2;dp[2][0][1]=1;dp[2][0][2]=1;
        dp[2][1][0]=3;dp[2][1][1]=1;dp[2][1][2]=0;

        for(int i=3;i<n+1;i++){
            dp[i][0][0] = ((dp[i-1][0][0]+dp[i-1][0][1])%mod+dp[i-1][0][2])%mod;
            dp[i][0][1] = dp[i-1][0][0] % mod;
            dp[i][0][2] = dp[i-1][0][1] % mod;

            int tmp = (dp[i-1][0][0]+dp[i-1][1][0])%mod;
            tmp = (tmp +dp[i-1][1][1]) %mod;
            tmp = (tmp +dp[i-1][0][2]) %mod;
            tmp = (tmp +dp[i-1][0][1]) %mod;
            tmp = (tmp +dp[i-1][1][2]) %mod;
            dp[i][1][0] = tmp;

            dp[i][1][1] = dp[i-1][1][0] % mod;
            dp[i][1][2] = dp[i-1][1][1] % mod;

        }

        int ans =0;
        for(int j=0;j<2;j++){
            for(int k=0;k<3;k++){
                ans = (ans +dp[n][j][k]) % mod;
            }
        }

        return ans;
    }
};
```