# A-785: Middle-Is-Graph-Bipartite

## 考点

* 二分图的判断
* 图的联通分量
> 这是这个题比较坑的点也是一直error的地方。__题目是需要每个联通分量都能成二分图，整个图才是二分图。__

## 题解

```cpp
//DFS染色法判断是否一个图是二分图
class Solution {
    private:
        std::vector<int> color;
public:
    bool isBipartite(vector<vector<int>>& graph) {
        color.resize(graph.size(),-1);
        for(int i=0;i<graph.size();i++){
            if(DFS(i,graph)) continue;
            return false;
        }
        return true;
    }

    bool DFS(int s,std::vector<std::vector<int>>& graph){

        std::stack<int> st;
        st.push(s); 
        if(color[s]==-1)
            color[s]=1;
        
        while(!st.empty()){
            auto cur_ind=st.top();st.pop();
            for(auto& ind:graph[cur_ind]){
                if(color[ind]==-1){
                    color[ind]=!color[cur_ind];
                    st.push(ind);
                }else if(color[ind]==color[cur_ind])
                    return false;
            }
        }
        return true;
    }
};
```
