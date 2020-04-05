# 876-Middle: Middle-of-the-Linked-List

## 考点

* 链表的访问
* 快慢指针

## 题解

### 快慢指针
```cpp
//这是自己的实现，非常丑，好的实现参看 /数据结构/快慢指针总结.md
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while(fast->next!=nullptr) {
            slow=slow->next;
            fast=fast->next;
            if(fast->next==nullptr) {break;}
            fast=fast->next;
        }

      return slow;
    }
};
```

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