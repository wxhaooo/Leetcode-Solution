# 261-Middle: Graph Valid Tree

## 考点

* 图与树的关系
> __一个图是树，当且仅当有n个顶点图有且只有n-1条边__
* 图的联通分量的个数
> 使用DFS/BFS来遍历记录联通分量的个数
* DFS/BFS

## 题解

```cpp
//先判断是否联通分量是1，如果不是1，则肯定不是树，可能是森林。
//如果联通分量是1，则判断边的个数是否是n-1，若是，则是树，否则有环，不是。
class Solution {
private:
    std::vector<bool> visited;
    std::vector<std::vector<int>> adj_table;
public:
    bool validTree(int n, vector<vector<int>>& edges) {
        visited.resize(n,false);
        adj_table.resize(n,vector<int>());
        for(auto&edge : edges){
            adj_table[edge[0]].push_back(edge[1]);
            adj_table[edge[1]].push_back(edge[0]);
        }

        int component=0;
        for(int i=0;i<n;i++){
            if(!visited[i]){
                component++;
                if(component>1) return false;
                DFS(i);
            }
        }

        return n-1==edges.size()?true:false;
    }

    void DFS(int cur){
        std::stack<int> st;
        st.push(cur);
        visited[cur]=true;
        while(!st.empty()){
            auto now = st.top();st.pop();
            for(auto& adj:adj_table[now]){
                if(!visited[adj]){
                     visited[adj]=true;
                     st.push(adj);
                }
            }
        }
    }
};
```