# 199-Middle: Binary Tree Right Side View

## 考点

* 层序遍历（广度优先搜索）
> 层序遍历有时候经常会对层进行设置考点，这个时候就要记录每个节点所在的层数，详见题解。这种方式还是非常常用的。

这道题乍一看似乎很奇怪，但是只要注意到从右往左看的第一个节点肯定是层序的最后一个节点，只要找到每层的最后一个节点即可。相似的，如果从左往右看只要找到每层的第一个节点即可。

## 题解

```cpp
//层序的记录层的写法
class Solution {
    private:
        std::vector<int> ans;
    public:
        vector<int> rightSideView(TreeNode* root) {
            if(root==nullptr) return std::vector<int>();
            //输出没层最右边的元素
            BFS(root);
            return ans;
        }

        void BFS(TreeNode* root)
        {
            std::queue<std::pair<TreeNode*,int>> q;
            q.push(std::make_pair(root,0));
            int prev_level=0;
            TreeNode* prev_node=root;
            while(!q.empty()){
                auto cur_info = q.front(); q.pop();
                auto cur_node = cur_info.first;
                int cur_level = cur_info.second;
                if(cur_level>prev_level){
                    ans.push_back(prev_node->val);
                    prev_level=cur_level;
                }
                prev_node = cur_node;

                if(cur_node->left!=nullptr) q.push(std::make_pair(cur_node->left,cur_level+1));
                if(cur_node->right!=nullptr) q.push(std::make_pair(cur_node->right,cur_level+1));
            }
            if(prev_node!=nullptr) ans.push_back(prev_node->val);
        }
    };
```