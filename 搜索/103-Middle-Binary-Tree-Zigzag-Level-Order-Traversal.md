# 103-Middle: Binary Tree Zigzag Level Order Traversal

## 考点

* BFS的变种

## 题解

```cpp
//这种才是层序的zig-zag访问，入队顺序一样，就是某些层数输出顺序需要翻转一样而已
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        std::vector<std::vector<int>> ans;
        if(!root)  return ans;
        std::deque<TreeNode*> dq;
        std::vector<int> local_ans;
        dq.push_front(root);
        bool reverse = false;
        while(!dq.empty()){
            local_ans.clear();
            int level_node_num = dq.size();
            for(int i=0;i<level_node_num;i++){
                if(!reverse)
                    local_ans.push_back(dq[i]->val);
                else
                    local_ans.push_back(dq[level_node_num -i -1]->val);
            }

            int cur_num =0;
            while(!dq.empty() && cur_num<level_node_num){
                auto cur_node = dq.front();
                if(cur_node->left)
                    dq.push_back(cur_node->left);
                if(cur_node->right)
                    dq.push_back(cur_node->right);
                dq.pop_front();
                cur_num++;
                }
            ans.push_back(local_ans);
            reverse = !reverse;
        }

        return ans;
    }
};
```

```cpp
//下面的代码是错误的，实现的是左右子树的zig-zag，而不是层序的zig-zag
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ans;
        if(!root)  return ans;
        std::queue<std::pair<TreeNode*,int>> q;
        int prev_level = 1;
        q.push(std::make_pair(root,1));
        ans.resize(1);
        while(!q.empty()){
            auto cur = q.front();
            q.pop();
            auto cur_node = cur.first;
            int cur_level = cur.second;

            if(cur_level == prev_level)
                ans[cur_level - 1].push_back(cur_node->val);
            else{
                ans.resize(ans.size()+1);
                ans[cur_level - 1].push_back(cur_node->val);
                prev_level = cur_level;
            }

            if(cur_level % 2==1){
                if(cur_node->right)
                    q.push(std::make_pair(cur_node->right,cur_level + 1));
                if(cur_node->left)
                    q.push(std::make_pair(cur_node->left,cur_level + 1));
            }
            else{
                if(cur_node->left)
                    q.push(std::make_pair(cur_node->left,cur_level + 1));
                if(cur_node->right)
                    q.push(std::make_pair(cur_node->right,cur_level + 1));
            }
        }
        return ans;
    }
};
```