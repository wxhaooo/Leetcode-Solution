# 22-Middle: Generate Parentheses

## 考点

* 回溯与剪枝
> 这道题只回溯然后判断是否括号序列合法也可以过，但是速度比较慢，剪枝可以大幅提高效率
* 动态规划
> 这道题还可以用动态规划去做，当然比较难想到，[参考这篇文章](https://leetcode-cn.com/problems/generate-parentheses/solution/zui-jian-dan-yi-dong-de-dong-tai-gui-hua-bu-lun-da/)

### 学到的东西

如果是在真的笔试面试的时候代码写的脏一点没关系，像这道题一开始就有个bug不太好调试，这个bug出现的原因就在于写的时候想精简代码。

## 题解

## 动态规划
```cpp
//动态规划的方法解决这道题，这个动态规划的方法是真的效率高，时间和内都是100%，非常厉害
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        if(n==0) return std::vector<string>();
        std::vector<std::vector<std::string>> dp(n+1);
        dp[0]=std::vector<std::string>(1,"");

        for(int i=1;i<dp.size();i++){
            for(int j=0;j<i;j++){
                for(auto& s1:dp[j]){
                    for(auto& s2:dp[i-1-j])
                        dp[i].push_back("("+s1+")"+s2);
                }
            }
        }
        return dp[n];
    }
};
```

### 回溯加剪枝

```cpp
//在任意状态，左括号数必须大于等于右括号数目。所以违反这个条件的都需要剪枝，剪枝后速度替身40倍，内存效率提升12倍左右
class Solution {
private:
    std::string local_ans;
    vector<string> ans;
public:
    vector<string> generateParenthesis(int n) {
        local_ans="";
        if(n==0) return std::vector<string>();
        DFSWithPrune(n*2,0,0);
        return ans;
    }

    void DFSWithPrune(int n,int left,int right){

        if(n==0){
            if(left==right)
                ans.push_back(local_ans);
            return;
        }

        if(left<right) return;

        local_ans+='(';
        DFSWithPrune(n-1,left+1,right);
        local_ans.pop_back();

        local_ans+=')';
        DFSWithPrune(n-1,left,right+1);
        local_ans.pop_back();
    }
};
```

### 只回溯的方法

```cpp
//只回溯的解法
class Solution {
private:
    std::string local_ans;
    vector<string> ans;
public:
    vector<string> generateParenthesis(int n) {
        local_ans="";
        if(n==0) return std::vector<string>();
        DFSWithPrune(n*2);
        return ans;
    }

    void DFSWithPrune(int n){

        if(n==0){
            if(IsValid(local_ans))
                ans.push_back(local_ans);
            return;
        }

        local_ans+='(';
        DFSWithPrune(n-1);
        local_ans.pop_back();

        local_ans+=')';
        DFSWithPrune(n-1);
        local_ans.pop_back();
    }

    bool IsValid(std::string& s){
        if(s.empty()) return false;
        std::stack<char> st;
        for(int i=0;i<s.size();i++){
            if(s[i]=='(')
                st.push(s[i]);
            else if(s[i]==')'&&!st.empty()&&st.top()=='(')
                st.pop();
            else
                return false;
        }
        return st.empty();
    }
};
```