# 207,210:Middle: Course Schedule I,II

这两道题因为类似，所以放在一起来理解了

## 考点

* __图的基本性质以及拓扑排序__
> 拓扑排序的实现有很多种，还是建议用indegree数组的方式

### 学到的东西

除了直接实现拓扑排序外，注意到拓扑排序只能针对DAG（有向无环图），所以还可以通过判断图中是否有环来确定是否可以进行拓扑排序。__判断一个图是否有环常用的方法是DFS，判断链表是否有环的方法是快慢指针法。__ 当然除了这种方法外，直接按拓扑排序的方法了来实现也不是不可以，但是速度比较堪忧。__注意这里使用的邻接表不是从A出去到达B所以table[A].add(B)，而是table[B].add(A),即记录的进入这个节点的邻接表__

## 题解

### DFS实现拓扑排序

```cpp
//详见刘汝佳算法竞赛入门指南第二版的168页
class Solution {
private:
    std::vector<int> c;
    std::vector<int> topo;
    int t;
    int n;
    std::vector<std::vector<int>> G;
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        n = numCourses;
        c.resize(numCourses);
        topo.resize(numCourses);
        G.resize(numCourses,std::vector<int>(numCourses,0));

        for(int i=0;i<prerequisites.size();i++)
            G[prerequisites[i][1]][prerequisites[i][0]] = 1;
        
        if(toposort())
            return topo;

        return std::vector<int>();
    }

    bool toposort(){
        t=n;
        for(int u=0;u<n;u++) if(!c[u])
        if(!DFS(u)) return false;
        return true;
    }

    bool DFS(int u){
        c[u]=-1;
        for(int v=0;v<n;v++) if(G[u][v]){
            if(c[v]<0) return false;
            else if(!c[v] && !DFS(v)) return false;
        }
        c[u]=1;topo[--t]=u;
        return true;
    }
   
};
```

### 使用入度表来改进

```cpp
//使用入度表来查哪个节点入度为0的改进版，速度是第一版的30倍左右，因为去掉了多次的remove,erase消耗
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        std::vector<int> ans;        
        std::vector<std::vector<int>> adj_table;
        std::vector<int> indegree(numCourses,0);
        adj_table.resize(numCourses);
        for(int i=0;i<prerequisites.size();i++){
            int in_node = prerequisites[i][0],out_node=prerequisites[i][1];
            adj_table[out_node].push_back(in_node);
            indegree[in_node]++;
        }
        
        int cur = -1,count=0;
        while(FindIndegree(indegree,cur))
        {
            //表示已经被访问过了
            indegree[cur]=-1;
            ans.push_back(cur);
            for(auto& e:adj_table[cur])
                indegree[e]--;
            count++;
        }

        if(count==numCourses)
            return ans;
      
        return std::vector<int>();
    }

    bool FindIndegree(std::vector<int>& indegree,int& cur)
    {
        for(int i=0;i<indegree.size();i++){
            if(indegree[i]==0){
                cur=i;
                return true;
            }
        }
        cur=-1;
        return false;
    }
};
```

### 原始版本

```cpp
//直接实现拓扑排序，即可以用来判断是否可以进行拓扑排序，也可以用来输出拓扑排序的序列，但是速度非常慢
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
         std::vector<std::vector<int>> adj_table;
         std::vector<int> ans;
        std::vector<bool> visited(numCourses,false);
        adj_table.resize(numCourses);
        for(int i=0;i<prerequisites.size();i++){
            int in_node = prerequisites[i][0],out_node=prerequisites[i][1];
            // adj_table[in_node].push_back(out_node);
            adj_table[in_node].push_back(out_node); 
        }

        bool find_remove_node=true;
        int count = 0;
        while(!adj_table.empty()&&find_remove_node){
            find_remove_node =false;
            for(int i=0;i<adj_table.size();i++){
                if(!visited[i]&&adj_table[i].empty()){
                    for(auto& entry:adj_table)
                        entry.erase(std::remove(entry.begin(),entry.end(),i),entry.end());
                    visited[i] = true;
                    find_remove_node = true;
                    ans.push_back(i);
                    count++;
                    break;
                }
            }
        }

        if(count==numCourses)
            return ans;
        return std::vector<int>();
    }
};
```