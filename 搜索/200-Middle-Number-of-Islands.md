# 200-Middle: Number of Islands

## 考点

* 搜索（DFS，BFS）


## 题解

```cpp
//搜索的简单写法
class Solution {
private:
    std::vector<std::pair<int,int>> dir={{-1,0},{1,0},{0,-1},{0,1}};

public:
    int numIslands(vector<vector<char>>& grid) {
        if(grid.size()==0) return 0;
        int m = grid.size(),n = grid[0].size();
        // std::cout<<m<<" "<<n;
        std::vector<std::vector<bool>> visited(m,std::vector<bool>(n,false));
        int ans = 0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]=='1' && !visited[i][j]){
                   BFS(i,j,grid,visited);
                    ans++;
                }
            }
        }
        return ans;
    }

    void BFS(int pos_x,int pos_y,vector<vector<char>>& grid,
    std::vector<std::vector<bool>>& visited)
    {
        int m = grid.size(),n = grid[0].size();
        std::queue<std::pair<int,int>> q;
        q.push(std::make_pair(pos_x,pos_y));
        visited[pos_x][pos_y] = true;
        while(!q.empty()){
            auto cur = q.front();
            q.pop();
            int cur_x = cur.first,cur_y = cur.second;

            for(int i=0;i<dir.size();i++){
                int cur_x = cur.first + dir[i].first;
                int cur_y = cur.second + dir[i].second;
                if(cur_x>=0 && cur_x<m && cur_y>=0 && cur_y<n
                && grid[cur_x][cur_y]=='1' && !visited[cur_x][cur_y]){
                    visited[cur_x][cur_y] = true;
                    q.push(std::make_pair(cur_x,cur_y));
                }
            }
        }
    }
};
```

```cpp
//题目目的就是为了找连通分类的数，一直遍历直到所有图可以访问的部分全部被访问完为止
//写的比较麻烦的一种
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        if(grid.size()==0) return 0;
        int m = grid.size(),n = grid[0].size();
        // std::cout<<m<<" "<<n;
        std::vector<std::vector<bool>> visited(m,std::vector<bool>(n,false));
        int ans = 0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]=='1' && !visited[i][j]){
                   BFS(i,j,grid,visited);
                    ans++;
                }
            }
        }
        return ans;
    }

    void BFS(int pos_x,int pos_y,vector<vector<char>>& grid,
    std::vector<std::vector<bool>>& visited)
    {
        int m = grid.size(),n = grid[0].size();
        std::queue<std::pair<int,int>> q;
        q.push(std::make_pair(pos_x,pos_y));
        visited[pos_x][pos_y] = true;
        while(!q.empty()){
            //这里cur不能用引用，因为后面马上pop()了，再一些比较严格的编译器会报错
            auto cur = q.front();
            q.pop();
            int cur_x = cur.first,cur_y = cur.second;

            if(cur_x - 1>=0 && !visited[cur_x -1][cur_y] && 
            grid[cur_x -1][cur_y]=='1'){
                q.push(std::make_pair(cur_x-1, cur_y));
                visited[cur_x -1][cur_y] = true;
            }

            if(cur_x +1 <m && !visited[cur_x +1][cur_y] &&
            grid[cur_x +1][cur_y]=='1'){
                q.push(std::make_pair(cur_x+1, cur_y));
                visited[cur_x +1][cur_y] = true;
            }

             if(cur_y - 1>=0 && !visited[cur_x][cur_y -1] &&
            grid[cur_x][cur_y -1]=='1'){
                q.push(std::make_pair(cur_x, cur_y -1));
                visited[cur_x][cur_y -1] = true;
            }

            if(cur_y +1 <n && !visited[cur_x][cur_y + 1] &&
            grid[cur_x][cur_y + 1]=='1'){
                q.push(std::make_pair(cur_x, cur_y + 1));
                visited[cur_x][cur_y + 1] = true;
            }
        }
    }
};
```
