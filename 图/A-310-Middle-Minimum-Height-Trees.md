# A-310-Middle: Minimum Height Trees

## 考点

* 图的基本概念
* 最短路算法（Floyd算法，超时）
* 拓扑排序变种
* __找树的重心__
> __两次dfs找到树的最长链，然后最长链上的中点一定就是树的重心。__

## 题解

### 拓扑排序变种

如果一个树的高度最小，那么从叶子开始删除，最后剩下的节点其实就是根。（这个点想不到，所以疯狂超时）

```cpp
//效率奇低的一般，2000多ms,54MB内存
class Solution {
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        std::vector<int> ans;
        if(n==2) {ans={0,1};return ans;}
       std::vector<std::vector<bool>> adj_mat(n,std::vector<bool>(n,false));
        std::vector<int> degree(n,0);
         std::vector<bool> remov(n,false);
       for(auto&edge : edges){
           int a = edge[0],b=edge[1];
           degree[a]++;degree[b]++;
           adj_mat[a][b]=adj_mat[b][a]=true;
       }

       int leaf_num = 0,left_num=n;
       while(true){
           leaf_num = 0;
           std::vector<int> tmp;
           for(int i=0;i<n;i++){
               if(degree[i]==1){
                   leaf_num++;
                    degree[i]--;
                    remov[i]=true;
                    tmp.push_back(i);
               }
           }
            for(auto&t:tmp){
            for(int j=0;j<n;j++){
                    if(adj_mat[t][j]){
                        adj_mat[j][t]=adj_mat[t][j]=false;
                        degree[j]--;
                    }
                }
            }

           left_num-=leaf_num;
           if(left_num==1 || left_num==2) break;
       }

      for(int i=0;i<n;i++){
          if(!remov[i]) ans.push_back(i);
      }
        return ans;
    }
};
```

### 求出任意两点的距离然后找最大的路径最为树高（结果超时）

```cpp
class Solution {
private:
    std::vector<int> ans;
    std::vector<std::vector<int>> adj_list;
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        int max_num = std::numeric_limits<int>::max();
      std::vector<std::vector<int>> dp(n,std::vector<int>(n,max_num));
      for(auto& edge:edges){
          dp[edge[0]][edge[1]]=1;
          dp[edge[1]][edge[0]]=1;
      }

        for(int i=0;i<n;i++) dp[i][i]=0;
    
    //flyod算法
      for(int i=0;i<n;i++){
          for(int j=0;j<n;j++){
              for(int k=0;k<n;k++){
                  if(dp[i][k]<max_num && dp[k][j]<max_num)
                  dp[i][j]=std::min(dp[i][j],dp[i][k]+dp[k][j]);
              }
          }
      }
        int min_height = std::numeric_limits<int>::max();
        for(int i=0;i<n;i++){
            int height = *std::max_element(dp[i].begin(),dp[i].end());
            if(height<min_height){
                ans.clear(); ans.push_back(i);
                min_height=height;
            }
            else if (min_height==height)
                ans.push_back(i);
        }

      return ans;
    }
};
```

### 列出所有树的高度然后找最小子树高度的根（结果超时）

```cpp
//DFS寻找树高然后判断是否是最小子树，结果超时
class Solution {
    private:
    std::vector<bool> visited;
    std::vector<int> ans;
    std::vector<std::vector<int>> adj_list;
public:
    vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
        adj_list.resize(n,std::vector<int>());

        int min_height = std::numeric_limits<int>::max();
        for(auto&edge:edges){
            adj_list[edge[0]].push_back(edge[1]);
            adj_list[edge[1]].push_back(edge[0]);
        }

        visited.resize(n,false);
        for(int i=0;i<n;i++){
            int cur_height = DFS(i);
            if(cur_height==min_height)
                ans.push_back(i);
            else if(cur_height<min_height){
                ans.clear();
                min_height = cur_height;
                ans.push_back(i);
            }
        }

        return ans;
    }

    int DFS(int cur){

        int max_height = 0;
        visited[cur]=true;

        for(auto& adj:adj_list[cur]){
            if(!visited[adj]){
                visited[adj]=true;
                int subtree_height=DFS(adj)+1;
                max_height = std::max(max_height,subtree_height);
                visited[adj]=false;
            }
        }
        visited[cur]=false;

        return max_height;
    }
};
```