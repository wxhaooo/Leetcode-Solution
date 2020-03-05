# 994-Simple: Rotting Oranges

## 考点

* BFS
> 像这种需要一圈一圈外扩的都是BFS，要是朝一个方向猛扑就是DFS。

## 题解

这道题实际上是个middle难度的题，关键在于处理一开始就有不同的烂橘子处于不同的位置的情况。这种情况 __只要一开始扫描一遍都入队且几位同一层即可。算是BFS的模板题了。__

### 学到的东西

虽然这种模板写的很熟悉了，但是还是会有bug，__主要问题总是出在少一个括号呀，if没加括号呀，变量名写错之类的，如果出现溢出或者越界问题要首先检查这种情况。__

```cpp
class Solution {
private:
    std::vector<std::pair<int,int>> dir{{0,1},{0,-1},{-1,0},{1,0}};
public:
    int orangesRotting(vector<vector<int>>& grid) {
        int max_days = 0;
        int m = grid.size(),n = grid[0].size();
        
        std::queue<std::tuple<std::pair<int,int>,int>> q;
        std::vector<std::vector<bool>> visited(m,std::vector<bool>(n,false));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==2){
                q.push(std::make_tuple(std::make_pair(i,j),0));
                visited[i][j] = true;
                }
            }
        }

        while(!q.empty()){
            std::pair<int,int> pos;
            int level;
            std::tie(pos,level) = q.front();
            max_days = std::max(max_days,level);
            q.pop();

            for(auto& d:dir){
                int dx = d.first,dy = d.second;
                int cur_x =pos.first + dx,cur_y = pos.second + dy;
                if(cur_x>=0 && cur_x<m && cur_y>=0 && cur_y<n && !visited[cur_x][cur_y] && grid[cur_x][cur_y]==1){
                    visited[cur_x][cur_y] = true;
                    grid[cur_x][cur_y] = 2;
                    q.push(std::make_tuple(std::make_pair(cur_x,cur_y),level+1));
                }
            }
        }

        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]==1 && !visited[i][j])
                    return -1;
            }
        }

        return max_days;
    }
};
```