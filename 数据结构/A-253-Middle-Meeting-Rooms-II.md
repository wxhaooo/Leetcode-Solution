# 253-Middle: Meeting Rooms II

## 考点

* 优先队列

## 学到的东西

* C++中的优先队列默认是大顶堆，如果需要使用小顶堆，使用自定义的排序函数且注意：__使用greater<...>函数来实现大顶堆__
* 不管是哪种题，暴力解都是最基础的，这道题能想到暴力解，但没想到暴力解的优化
* 不一定分类是啥题就用啥方法做，leetcode这点够恶心的

## 题解

[优先队列参看这个题解](https://leetcode-cn.com/problems/meeting-rooms-ii/solution/hui-yi-shi-ii-by-leetcode/)
```cpp
//优先队列的解法
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        if(intervals.size()==0) return 0;
        std::sort(intervals.begin(),intervals.end(),
        [](const std::vector<int>& e1,const std::vector<int>& e2)->bool
        {
            return e1[0]<e2[0];
        }
        );
        std::priority_queue<int,std::vector<int>,std::greater<int>> p_queue;
        p_queue.push(intervals[0][1]);

        for(int i=1;i<intervals.size();i++){
            if(intervals[i][0]>=p_queue.top())
                p_queue.pop();
            p_queue.push(intervals[i][1]);
        }

        return p_queue.size();
    }
};
```