# 28-Simple:Implement strStr()

## 考点

* 字符串匹配
> 这个题很神奇的一点是可以用各语言内置的字符串匹配去做。不要还是学习下考点KMP算法为好

## 题解

```cpp
//神经病解法，直接用std::strstr解决。。。
class Solution {
public:
    int strStr(string haystack, string needle) {
        if(needle.size()==0) return 0;
        auto ptr = std::strstr(haystack.c_str(),needle.c_str());
        if(ptr==nullptr)
            return -1;
        return (ptr - haystack.data())/sizeof(char);
    }
};
```
```cpp
//手写KMP算法来解决，即手写strstr()
```