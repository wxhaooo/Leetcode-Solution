# 114-Middle: Flatten Binary Tree to Linked List

## 考点

* 树的遍历
> 虽然所有的遍历都很好写，但是往往想不到怎么按照题目中指定的方法去遍历，__这道（类）题建议直接记忆即可__

## 题解

```cpp
//因为右子树不能被删除，所以要使用后序遍历的方法
class Solution {
public:
    void flatten(TreeNode* root) {
        PostOrderTraversal(root);
    }

    void PostOrderTraversal(TreeNode* cur_node){
        if(cur_node==nullptr) return;
        PostOrderTraversal(cur_node->left);
        PostOrderTraversal(cur_node->right);

        //先把原来的右子树存下来
        TreeNode *tmp = cur_node->right;
        //把左子树当成新的右子树
        cur_node->right = cur_node->left;
        //原来的左子树置空
        cur_node->left = nullptr;
        //一直找到右子树最后一个右子女，然后把原来的左子女给它即可
        while(cur_node->right!=nullptr) cur_node=cur_node->right;
        cur_node->right = tmp;
    } 
};
```