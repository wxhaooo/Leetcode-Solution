# 130-Middle: Surrounded Regions

## 考点

* BFS/DFS的变种
* __并查集__
> 并查集是一个新思路，__并查集可以用来合并不同的连通区域，简单来说就是把图先序列化成数字，对图进行遍历，然后把符合连通条件的图合并在一起，最后的根节点数目也反应了图中有多少连通区域。__
>以后遇到类似的问题可以试试并查集的方法。

## 题解

这道题的难点在于触碰到边界的一次搜索会导致整个搜索树重置，所以需要一边搜索一遍保存搜索过的位置，当搜索碰到边界时再恢复。另外当碰到边界后，搜索对于元素处理的行为也会发生变化，所以需要蛇者一个标记has_board来改变搜索的行为。

### 学到的东西

考点没有什么新的东西，但是调bug着实调了很久。__对于这种题建议先在纸上写好思路，最后调bug也好调些。另外这个题也可能当成一个模板了，对于变量的命名也固定一下，调bug时也好调些。__

```cpp
//BFS的方法
class Solution {
private:
    int m,n;
    std::vector<std::pair<int,int>> dir{{-1,0},{1,0},{0,1},{0,-1}};
public:
    void solve(vector<vector<char>>& board) {
        if(board.size()==0) return;
        m = board.size();n = board[0].size();
        std::vector<std::vector<bool>> visited(m,std::vector<bool>(n,false));
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(!IsBoard(i,j)&&!visited[i][j]&&board[i][j]=='O'){
                    BFS(i,j,board,visited);
                }
            }
        }
    }

    void BFS(int pos_x,int pos_y,
        vector<vector<char>>& board,
        std::vector<std::vector<bool>>& visited)
    {
        std::queue<std::pair<int,int>> q;
        std::queue<std::pair<int,int>> recover;
        q.push(std::make_pair(pos_x,pos_y));
        recover.push(std::make_pair(pos_x,pos_y));
    
        visited[pos_x][pos_y]=true;
        board[pos_x][pos_y]='X';
        bool has_board=false;

        while(!q.empty()){
            auto pos = q.front(); q.pop();
            int cur_x = pos.first,cur_y = pos.second;
            if(!has_board && IsBoard(cur_x, cur_y)){
                has_board =true;
                while(!recover.empty())
                {
                    auto rc_pos =recover.front();
                    recover.pop();
                    board[rc_pos.first][rc_pos.second]='O';
                }
            }

            for(auto& d:dir){
                int dx =d.first,dy =d.second;
                int new_x=cur_x+dx,new_y=cur_y+dy;

                if(!has_board)
                {
                    if(!visited[new_x][new_y]&&board[new_x][new_y]=='O')
                    {
                        visited[new_x][new_y]=true;
                        board[new_x][new_y]='X';
                        q.push(std::make_pair(new_x,new_y));
                        recover.push(std::make_pair(new_x,new_y));
                    }
                }
                else
                {
                    if(new_x>=0&&new_x<m&&new_y>=0&&new_y<n 
                    && !visited[new_x][new_y] && board[new_x][new_y]=='O')
                    {
                        visited[new_x][new_y]=true;
                        q.push(std::make_pair(new_x,new_y));
                    }
                }
            }
        }
    }

    bool IsBoard(int i,int j)
    {
         if(i==0||i==m-1||j==0||j==n-1)
            return true;
        return false;
    }
};
```