# 104-Simple: Maximum Depth of Binary Tree

## 考点

* 树的遍历
* 搜索算法（BFS，DFS）

##题解

```cpp
 //简单的递归解法，深度过深有爆栈的风险，不过leetcode上也能过
class Solution {
public:
    int maxDepth(TreeNode* root) {
        return MaxDepth(root);
    }

    int MaxDepth(TreeNode* node)
    {
        if(node==nullptr) return 0;

        return std::max(MaxDepth(node->left),MaxDepth(node->right)) + 1;
    }
};
```

```cpp
//BFS的方法解决问题，没有爆栈的风险，使用DFS也能完成同样的功能
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root == nullptr) return 0;
        std::queue<std::pair<TreeNode*,int>> q;
        q.push(std::make_pair(root,1));
        int max_depth =0;
        while(!q.empty()){
            auto now = q.front();
            q.pop();
            auto now_node = now.first;
            auto now_depth = now.second;
            max_depth = std::max(max_depth,now_depth);
            if(now_node->left!=nullptr)
                q.push(std::make_pair(now_node->left,now_depth+1));
            if(now_node->right!=nullptr)
                q.push(std::make_pair(now_node->right,now_depth+1));
        }
        return max_depth;
    }
};
```