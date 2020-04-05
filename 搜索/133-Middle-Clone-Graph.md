# 133-Middle: Clone Graph

## 考点

* 深度优先搜索
* 深度拷贝

## 题解

这道题本身不难，难在这憨憨题目一时人还看不懂是啥意思，看懂了就非常简单。

```cpp
//DFS实现的深度拷贝
class Solution {
private:
        std::unordered_map<int,Node*> map;
public:
    Node* cloneGraph(Node* node) {
        if(node==nullptr) return nullptr;
         Node* ans = nullptr;
         //找不到建立好的结点就新建一个
        if(map.find(node->val)==map.end()){
            ans = new Node();
            ans->val = node->val;
            ans->neighbors = node->neighbors;

            map[ans->val]=ans;
        }else
        //否则直接拿就好
            return map[node->val];
        //如果是新建的，则看看所有的邻居是不是也要拷贝
        for(auto& neigh:ans->neighbors){
            neigh=cloneGraph(neigh);
        }
        return ans;
    }
};
```