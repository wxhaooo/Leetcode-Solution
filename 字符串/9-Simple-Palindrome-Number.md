# 9-Simple:Palindrome-Number

## 考点

* 字符串处理

## 题解

[这个的第三种解法很有意思，可以学习下](https://leetcode-cn.com/problems/palindrome-number/solution/dong-hua-hui-wen-shu-de-san-chong-jie-fa-fa-jie-ch/)

```cpp
//简单解法
class Solution {
public:
    bool isPalindrome(int x) {
        if(x<0) return false;
        std::vector<int> tmp;
        while(x){
            tmp.push_back(x%10);
            x/=10;
        }
        for(int i=0;i<tmp.size()/2;i++){
            if(tmp[i]!=tmp[tmp.size()-1-i]) return false;
        }
        return true;
    }
};
```