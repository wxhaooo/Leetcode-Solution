# A-面试题59-Middle: II. 队列的最大值 LCOF

## 考点

* 摊还成本
> 这道题要求各种操作是$O(1)$也是相当的骚气，最后看了答案才知道是摊还的...

## 题解

```cpp
//push_back看起来是O(n)的，但是因为每一个数都只push_back和push_front一次，摊还下来就是O(1)
class MaxQueue {
private:
    std::deque<int> data_;
    std::deque<int> aux_;

public:
    MaxQueue() {
    }
    
    int max_value() {
        if(data_.empty()) return -1;
        return aux_.front();
    }
    
    void push_back(int value) {
       data_.push_back(value);
       while(!aux_.empty()){
           int tmp = aux_.back();
           if(value < tmp)
                break;
                aux_.pop_back();
       }
        aux_.push_back(value);
    }
    
    int pop_front() {
        if(data_.empty()) return -1;
       if(data_.front()==aux_.front())
            aux_.pop_front();
        int res = data_.front();
        data_.pop_front();
        return res;
    }
};

/**
 * Your MaxQueue object will be instantiated and called as such:
 * MaxQueue* obj = new MaxQueue();
 * int param_1 = obj->max_value();
 * obj->push_back(value);
 * int param_3 = obj->pop_front();
 */
```