# 7-Simple: Reverse Integer

## 考点

* __溢出的判断__ 和语言基础

### 学到的东西

这道题其实可以学到一个重要的知识点：__int类型的溢出判断__ 这种判断之所以重要是 __因为有些编译器(gcc)在数溢出的时候直接按error处理，没法用符号翻转的方法判断溢出。__

原理参见[Int类型的溢出检测](https://leetcode-cn.com/problems/reverse-integer/solution/zheng-shu-fan-zhuan-by-leetcode/)，
注意这里的除法是int除法，而$7$,$8$分别是int最大值最小值的个位($2^{31}-1=2147483647,-2^{31}=-2147483648$），所以有了那个判断条件。

__这段代码最好背过，有用的地方比较多。__

## 题解

```cpp
//这种通过求余的方式来得到每一位其实是正负通吃的，所以不需要额外对符号
class Solution {
public:
    int reverse(int x) {
        int max = 0x7fffffff, min = 0x80000000;
        long rs = 0;
        for(;x;rs = rs*10+x%10,x/=10);
        return rs>max||rs<min?0:rs;
    }
};
```