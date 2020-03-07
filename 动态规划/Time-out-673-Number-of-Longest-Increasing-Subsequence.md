# Time-out-673: Number of Longest Increasing Subsequence

## 考点

* 两个数组的动态规划

## 题解

```cpp
//使用两个数组来动态规划
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        if(nums.size()==0) return 0;
        if(nums.size()==1) return 1;
       std::vector<int> dp(nums.size(),1);
       std::vector<int> count(nums.size(),1);

       int max_len = 0,ans=0;
       for(int i=1;i<nums.size();i++){
           for(int j=0;j<i;j++){
               if(nums[i]>nums[j]){
                   if(dp[j]+1>dp[i]){
                       count[i] = count[j];
                       dp[i]=std::max(dp[i],dp[j]+1);
                   }
                   else if(dp[j]+1==dp[i]){
                       count[i]+=count[j];
                   }
               }
           }
           max_len = std::max(max_len,dp[i]);
       }

    for(int i=0;i<count.size();i++){
        // std::cout<<count[i]<<endl;
        if(dp[i]==max_len)
            ans+=count[i];
    }
      
      return ans;
    }
};
```

```cpp
//优化版，还是超时
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        if(nums.size()==0) return 0;

        std::vector<std::vector<long long>> dp(nums.size(),std::vector<long long>(nums.size()+1,0));

        for(int i=0;i<nums.size();i++)
            dp[i][1] = 1;

        int ans_len = 0,ans=0;
        for(int i=1;i<nums.size();i++){
            for(int j=1;j<=i+1;j++){
                for(int k=0;k<i;k++){
                    if(nums[k]<nums[i]){
                        dp[i][j]+=dp[k][j-1];
                    }        
                }
            }
        }

         for(int l=nums.size();l>=0;l--){
            long long ans = 0;
            for(int i=0;i<nums.size();i++)
                ans += dp[i][l];
            if(ans>0)
                return ans;
        }       

        return ans;
    }
};
```

```cpp
//结果正确但是超时，动态规划搞的太复杂了
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        if(nums.size()==0) return 0;

        std::vector<std::vector<long long>> dp(nums.size(),std::vector<long long>(nums.size()+1,0));
        for(int i=0;i<nums.size();i++)
            dp[i][1] = 1;
        for(int i=1;i<nums.size();i++){
            for(int j=1;j<=nums.size();j++){
                for(int k=0;k<i;k++){
                    if(nums[k]<nums[i]){
                        dp[i][j]+=dp[k][j-1];
                    }        
                }
            }
        }

        for(int l=nums.size();l>=0;l--){
            long long ans = 0;
            for(int i=0;i<nums.size();i++)
                ans += dp[i][l];
            if(ans>0)
                return ans;
        }        
        return 0;
    }
};
```