# 125-Simple: Valid Palindrome

## 考点

* 加强版的回文数判断
* 常用字符串处理函数
> 判断/转换大小写，判断是否是数字字母等（见字符串总结）

## 题解

```cpp
//自己的解法比较蠢，主要是看了题意以为不考虑数字，结果
//有的例子就有数字，后面修修补补就有了下面的代码
class Solution {
public:
    bool isPalindrome(string s) {
        while(start<s.size() && !std::isdigit(s[start]) && !std::isalpha(s[start])){start++;}
        while(end>0 && !std::isdigit(s[end]) && !std::isalpha(s[end])){end--;}
        // std::cout<<start<<" "<<end<<" ";
        while(start<end){
            if(std::isdigit(s[start])&&!std::isdigit(s[end])||
            !std::isdigit(s[start])&&std::isdigit(s[end])||
             std::tolower(s[start])!=std::tolower(s[end]))
                return false;
            start++;
            end--;
            while(start<s.size() && !std::isdigit(s[start]) && !std::isalpha(s[start])){start++;}
            while(end>0 && !std::isdigit(s[end]) && !std::isalpha(s[end])){end--;}
            // std::cout<<start<<" "<<end<<" ";
        }
        return true;
    }
};
```

```cpp
//这是一种考虑到数字的方法，代码就比较简洁
class Solution {
public:
    bool isPalindrome(string s) {
        int start = 0,end =s.size() -1;
        while(start<=end){
            if(!std::isalnum(s[start])) {start++;continue;}
            if(!std::isalnum(s[end])) {end--;continue;}
            if(std::tolower(s[start])!=std::tolower(s[end])) return false;
            start++;end--;
        }
        return true;
    }
};
```