# 5-Middle: Longest Palindromic Substring

## 备注

* 第二次做也没做出来

## 考点

* 看着是考字符串，但是实际考的是是动态规划
* 扩展中心法也可以做这个题
* Manacher's Algorithm也可以做

__这个题除了暴力解法，其他解法都想不到...，所以这个题的所有解法都要好好学习。__

## 题解

```cpp
//DP的状态方程是d[i][j] = d[i-1][j-1] + 1
//d[i][j]表示对于串s1,s2,s1[0~i],s2[0~j]最长的公共子串是多长，又字符串知道结尾索引是i，长度是d[i][j]，所以可以得到子串的起始位置s = i - d[i][j] + 1.
class Solution {
public:
    string longestPalindrome(string s) {
        if(s.size()==0) return std::string("");
       //d[i][j] = d[i-1][j-1] + 1
       std::array<std::array<int,1005>,1005> d{0};
        std::string s_rev =s;
        std::string ans = "";
        int cur_max_len = -1;
        int end = -1;
        std::reverse(std::begin(s_rev),std::end(s_rev));
        // std::cout<<s_rev;

        for(int i=0;i<s.size();i++){
            for(int j=0;j<s_rev.size();j++){
                if(s[i]==s_rev[j]){
                    if(i==0 || j==0) d[i][j] = 1;
                    else d[i][j] = d[i - 1][j - 1] + 1;
                }
                if(d[i][j]>cur_max_len){
                int beforeRev = s.size() - 1 - j;
                if (beforeRev + d[i][j] - 1 == i) { 
                   cur_max_len = d[i][j];
                    end = i;
                }
            }
            }
        }
        ans = s.substr(end-cur_max_len+1,cur_max_len);

        return ans;
        
    }
};
```

```cpp
//扩展中心法，利用回文串一定是中心对称的。思路非常简单也好实现，可以学习到已知长度和中心怎么算起始和结束
class Solution {
public:
    string longestPalindrome(string s) {
        if(s.size()==0) return std::string("");
        int max_len = -1;
        int ans_i = -1;
        for(int i=0;i<s.size();i++) {
            int len_1 = ExpendAround(i, i, s);
            int len_2 = ExpendAround(i, i + 1, s);
            int len = std::max(len_1,len_2);
            if(len > max_len){
                max_len = len;
                ans_i = i - (len-1)/2; 
            }
        }
        return s.substr(ans_i,max_len);
    }
private:
    int ExpendAround(int start,int end,std::string& s)
    {
        int L = start,R = end;
        for(;L>=0 && R<s.size() && s[L]==s[R];L--,R++){}
        return R-L-1;
    }
};
```