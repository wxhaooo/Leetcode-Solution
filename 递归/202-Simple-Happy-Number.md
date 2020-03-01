# 202-Simple: Happy Number

## 考点

* 链表判环
> 解决方法有递归和 __快慢指针__

### 可以学到的东西

这个快慢指针有点神奇，可以用来判定链表是否是循环链表，证明见[为什么用快慢指针找链表的环，快指针和慢指针一定会相遇？](https://www.zhihu.com/question/23208893)

## 题解

```cpp
//快慢指针法
class Solution {
public:
    bool isHappy(int n) {
       int fast=n,slow = n;
       do
       {
           slow = next(slow);
           fast = next(fast);
           fast = next(fast);
       }while(fast!=slow);

       return slow == 1;
    }
private:
    int next(int n)
    {
        int tmp = 0;
        while(n)
        {
            tmp+=((n%10)*(n%10));
            n/=10;
        }
        return tmp;
    }
};
```

```cpp
//通过记忆递归中的值来找到是否循环了，效率比较低
class Solution {
private:
    std::vector<int> mem;
public:
    bool isHappy(int n) {
        mem.push_back(n);
        return IsHappy(n);
    }
private:
    bool IsHappy(int& n)
    {
        int tmp_n =0;
        while(n>0)
        {
            int tmp = n%10;
            tmp_n+=(tmp * tmp);
            n/=10;
        }
    
        if(tmp_n==1) return true;
        if(std::find(mem.begin(),mem.end(),tmp_n)!=mem.end()) return false;
        mem.push_back(tmp_n);
        

        return IsHappy(tmp_n);
    }
};
```