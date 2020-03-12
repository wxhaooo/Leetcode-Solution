# 1071-Simple: Greatest Common Divisor of Strings

## 考点

* 欧几里得算法的扩展

> 首先欧几里得算法忘掉了，第二这个扩展实在有点巧妙。好在暴力枚举一波剪剪枝也倒是能过...


## 题解

### 欧几里得算法

```cpp
//性能是暴力枚举的四倍，不得不说这个方法有点吊的飞起
class Solution {
public:
    string gcdOfStrings(string str1, string str2) {
        if(str1+str2!=str2+str1) return string("");
        return str1.substr(0,gcd(str1.size(),str2.size()));
    }

    int gcd(int a,int b){if(b==0) return a; return gcd(b,a%b);}
};
```

### 暴力枚举

```cpp
//从短的字符串开始枚举，截断一些不必要的枚举
class Solution {
public:
    string gcdOfStrings(string str1, string str2) {
        if(str1.empty()||str2.empty()) return std::string("");
       if(str1.size()<str2.size()) std::swap(str1,str2);

        for(auto len=str2.size();len>0;len--){
           auto tmp=str2.substr(0,len);
           int pos=0;
           for(;str2.find(tmp,pos)!=string::npos;pos+=tmp.size()){}
           if(pos!=str2.size()) continue;
           for(pos=0;str1.find(tmp,pos)!=string::npos;pos+=tmp.size()){}
           if(pos==str1.size()) return tmp;
        }
        return std::string("");
    }
};
```