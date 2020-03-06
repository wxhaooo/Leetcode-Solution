# 323-Middle: Number of Connected Components in an Undirected Graph

## 考点

* DFS/BFS查找连通分量
* 并查集查找连通分量

## 题解

```cpp
//并查集查找连通分量，并查集封装成类作为模板
class DisjoinedSet
{
    public:
        DisjoinedSet() =delete;
        DisjoinedSet(int n){
            par_.resize(n); rank_.resize(n);
            for(int i=0;i<n;i++){
                par_[i] = i;
                rank_[i] = 0;
            }
        }
    
    public:
        int Find(int x){
            if(par_[x]==x) return x;
            else return  par_[x]=Find(par_[x]);
        }

        void Unite(int x,int y){
            x = Find(x);
            y = Find(y);

            if(x==y) return;

            if(rank_[x]<rank_[y]) par_[x] = y;
            else{
                 par_[y]=x;
                 if(rank_[x]==rank_[y]) rank_[x]++;
            }
        }

        bool IsSame(int x,int y){
            return Find(x)==Find(y);
        }

    private:
        std::vector<int> par_;
        std::vector<int> rank_;
    
};
class Solution {
public:
    int countComponents(int n, vector<vector<int>>& edges) {
       DisjoinedSet disjoined_set(n);
       for(auto& edge:edges)
           disjoined_set.Unite(edge[0], edge[1]);
        
        std::unordered_set<int> ans;
        for(int i=0;i<n;i++)
            ans.insert(disjoined_set.Find(i));
        
        return ans.size();
    }
};
```