# 344-Simple: Reverse String

##考点

* 字符串翻转

题目超简单，但是可以思考下字符串文本超大的情况（40G的文本怎么翻转之类的）
> 不能直接放入内存的文本就分段装进内存然后翻转再拼接即可。

```cpp
//只用运行一般的位置即可
class Solution {
public:
    void reverseString(vector<char>& s) {
        // std::reverse(s.begin(),s.end());
        for(int start=0,end=s.size()-1;start<s.size()/2;start++){
            std::swap(s[start],s[end-start]);
        }
    }
};
```