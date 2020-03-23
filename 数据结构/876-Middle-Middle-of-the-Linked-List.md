# 876-Middle: Middle-of-the-Linked-List

## 考点

* 链表的访问
* 快慢指针

## 题解

### 快慢指针

### 访问计数
```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* tmp = head;
        int len = 0;
        while(tmp!=nullptr){len++;tmp=tmp->next;}
        int mid = std::ceil(len/2);
        for(int i=0;i<mid;i++)  head=head->next;

        return head;
    }
};
```