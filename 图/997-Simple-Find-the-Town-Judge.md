# 997-Simple: Find the Town Judge

## 考点

* 图的基本概念（出入度）

## 题解

法官肯定是出度为0，入度为1的点，也没必要使用邻接矩阵和邻接表这种大型数据结构，直接用数组记录即可。时间空间复杂度均为$O(N)$

```cpp
class Solution {
public:
    int findJudge(int N, vector<vector<int>>& trust) {
        if(N==0) return -1;
        std::vector<std::pair<int,int>> table(N,{0,0});
        int m = trust.size();
        for(int i=0;i<m;i++){
                table[trust[i][0]-1].first++;
                table[trust[i][1]-1].second++;
        }
        for(int i=0;i<N;i++){
            if(table[i].first==0 && table[i].second==N-1)
                return i+1;
        }
        return -1;
    }
};
```
