# 102-Simnple: Binary Tree Level Order Traversal

## 考点 

* 树的遍历方法

### 能学到的东西

这个题都是套路题，考点无非是DFS，BFS以及树的三种遍历方法加一种左子树右子树分治方法。除了掌握DFS,BFS的递归非递归方法外，也 __要学会用DFS，BFS判断现在在哪一层。__ `一个通用的方法就是压栈或者入队时把层的编号一起压进去。`

## 题解

```cpp
//BFS的方法来完成层序遍历
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        std::vector<std::vector<int>> ans;
        if(root==nullptr) return ans;
        std::queue<std::pair<TreeNode*,int>> q;
        ans.resize(1);

        q.push(std::make_pair(root,1));
        int prev_level =1;

        while(!q.empty()){
            auto cur = q.front();
            q.pop();
            auto cur_node = cur.first;
            int cur_level = cur.second;
            if(cur_level == prev_level)
                ans[cur_level -1].push_back(cur_node->val);
            else{
                ans.resize(ans.size() + 1);
                ans[cur_level -1].push_back(cur_node->val);
                prev_level = cur_level;
            }

            if(cur_node->left)
                q.push(std::make_pair(cur_node->left,cur_level+1));
            if(cur_node->right)
                q.push(std::make_pair(cur_node->right,cur_level+1));
        }

        return ans;
    }
};
```