# 105-Middle: Construct Binary Tree from Preorder and Inorder Traversal

## 考点

* 树的重建（任意两种遍历方式可以重建整个树）
* 分治与递归

### 学到的东西

分治的写法总结：要能把大问题分成小问题，__写分治的时候先写分，再写治，最后补充边界条件。__

## 题解

```cpp
//效率比较低，思路比较复杂的一个分治方法
//主要依赖以下几点：
//1. 先序的第一个一定是根
//2. 在中序里找到这个根，则这个根把中序序列分成了两半，一半是左子树，一半是右子树
//3. 既然知道左右子树是两半，那容易得到左右子树的大小，又知道
//先序的头是根，那么从根以后都是左右子树，又知道两棵树的大小，那很容易得到两棵树的先序范围
//由此不断分治就得最终结果
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if(preorder.size()==0 || inorder.size()==0) return nullptr;
      return BuildSubTree(0,inorder.size()-1,0,preorder.size()-1,preorder,inorder);
    }
private:
    TreeNode* BuildSubTree(int in_start,int in_end,int pre_start,int pre_end,
                        vector<int>& preorder,vector<int>& inorder)
    {
        TreeNode* root = nullptr;
        if(in_start > in_end)
            return root;
        if(in_start == in_end){
            root = new TreeNode(inorder[in_start]);
            root->left = root->right = nullptr;
            return root;
        }

        auto in_iter = std::find(inorder.begin(),inorder.end(),preorder[pre_start]);
        int root_pos =  std::distance(inorder.begin(),in_iter);

        root = new TreeNode(inorder[root_pos]);

        int left_tree_size = root_pos - in_start;
        int right_tree_size = in_end - root_pos;

        root->left = BuildSubTree(in_start,root_pos-1,pre_start+1,pre_start+left_tree_size,preorder,inorder);
        root->right = BuildSubTree(root_pos+1,in_end,pre_start+1+left_tree_size,pre_end,preorder,inorder);

        return root;
    }
};
```