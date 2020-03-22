# 945-Middle:Minimum Increment to Make Array Unique

## 考点

* 枚举遍历即可，但是枚举也有技巧，造成最后的运行效率不同
* 枚举结合排序可以降低算法复杂度
* 如果使用计数排序复杂度可以降低到$O(n)$
* 路径压缩

### 学到的东西

__这道题最能学到的东西就是路径压缩__，虽然枚举的方法想到了，但是没想到路径压缩，但是这种并查集中用到的技巧看起来适用面还是挺广的。

## 题解

### 计数排序

计数排序也可以解决这个问题，这里不记录代码，因为计数排序用的很少，放一个题解作为参考
[参看计数排序方法的题解](https://leetcode-cn.com/problems/minimum-increment-to-make-array-unique/solution/ji-shu-onxian-xing-tan-ce-fa-onpai-xu-onlogn-yi-ya/)

### 路径压缩

利用并查集路径压缩的思路，算法复杂度降低到$O(n)$,[参看路径压缩方法的题解](https://leetcode-cn.com/problems/minimum-increment-to-make-array-unique/solution/ji-shu-onxian-xing-tan-ce-fa-onpai-xu-onlogn-yi-ya/)

__这个思路要重点掌握，要会默写。__


```cpp
//利用了并查集路径压缩的思路
class Solution {
    private:
        std::vector<int> table;

        int find(int pos){
            int aa=table[pos];
            if(aa==-1){
                table[pos]=pos;
                return pos;
            }

            aa = find(aa+1);
            table[pos]=aa;
            return aa;
        }

    public:
        int minIncrementForUnique(vector<int>& A) {
            int ans =0;
            table.resize(80000+5,-1);
            for(auto& ae:A){
                ans+=(find(ae)-ae);
            }
            return ans;
        }
};
```

### 排序枚举

```cpp
//先排序，然后记录当前位置前面的最大值，当出现重复时只要移动到最大值+1即可，当后面遇到最大值+1时再按同样的方法处理即可，复杂度O(n logn)
class Solution {
public:
    int minIncrementForUnique(vector<int>& A) {
        int ans =0;
        std::sort(A.begin(),A.end());
        std::unordered_set<int> table;
        int cur_max = -1;
        for(auto& aa:A){
            cur_max = std::max(aa,cur_max);
            if(table.find(aa)==table.end()){
                table.insert(aa);
                continue;
            }

            table.insert(cur_max+1);
            cur_max = cur_max+1;
            ans+=(cur_max-aa);
        }

        return  ans;
    }
};
```

### 暴力枚举

```cpp
//暴力枚举,复杂度O(n^2)
class Solution {
public:
    int minIncrementForUnique(vector<int>& A) {
        std::vector<int> table(80000+5,0);
        for(auto& aa:A) table[aa]++;
        int ans=0;
       for(int i=0;i<table.size();i++){
           int local_ans=0;
           while(table[i]>1){
                int tmp =i;
                while(table[tmp]!=0){tmp++;}
                table[tmp]++;
                table[i]--;    
                local_ans+=tmp-i;
           }
           ans+=local_ans;
       }
       return ans;
    }
};
```