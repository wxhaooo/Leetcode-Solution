# 19-Middle:Remove-Nth-Node-From-End-of-List

## 考点

* 快慢指针访问链表的改造版

## 题解

```cpp
//一次遍历，使用一个table存储访问过的指针
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* hhead=head;
        std::vector<ListNode*> tmps;
        while(hhead!=nullptr){tmps.push_back(hhead);hhead=hhead->next;}
        int nn = tmps.size()-n;
        if(nn==0) {head=head->next;delete tmps[nn];}
        else if(nn==tmps.size()-1) {tmps[nn-1]->next=nullptr;delete tmps[nn];}
        else{ tmps[nn-1]->next=tmps[nn]->next;delete tmps[nn];}
        return head;
    }
};
```