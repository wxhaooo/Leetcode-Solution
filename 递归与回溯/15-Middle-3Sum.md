# 15-Middle-3Sum

## 考点

* 双指针，排序
* 递归搜索

## 题解

### 双指针+排序
根据提示，可以先确定一个数，然后使另外两个数的和为这个数。两和问题可以用双指针在$O(n)$的时间解决，则这种方法的整体时间复杂度为$O(n^2)$，暴力解法$O(n^3)$，最后结果也是可以过的。需要注意去重处理，首先是选的第一个数不能重复，所以用一个table来记录，另一个是两和不能重复，所以需要跳过这些重复的元素组。

```cpp
//不使用table来记录的话性能有小幅度上升
class Solution {
private:
    std::vector<std::vector<int>> ans;
public:
    std::vector<std::vector<int>> threeSum(std::vector<int>& nums) {
        if (nums.size() == 0 || nums.empty()||nums.size()<3) return ans;
        std::sort(nums.begin(), nums.end());
        // std::set<int> table;
        for(int i=0;i<nums.size();i++){
            while(i>0&& i<=nums.size()-1 && nums[i]==nums[i-1]) i++;
            // if(table.find(nums[i])!=table.end()) continue;
            // table.insert(nums[i]);
            int start=i+1,end=nums.size()-1;
            while(start<end){
                if(nums[start]+nums[end]+nums[i]>0) end--;
                else if(nums[start]+nums[end]+nums[i]<0) start++;
                else {
                    std::vector<int> local_ans{nums[i],nums[start],nums[end]};   
                    ans.push_back(local_ans);
                    start++;end--;
                    while(start<end && nums[start]==nums[start-1])start++;
                    while(start<end && nums[end]==nums[end+1])end--;
                    }
                }
            }
             return ans;
        }
};
```

```cpp
class Solution {
private:
    std::vector<std::vector<int>> ans;
public:
    std::vector<std::vector<int>> threeSum(std::vector<int>& nums) {
        if (nums.size() == 0 || nums.empty()) return ans;
        std::sort(nums.begin(), nums.end());
        std::set<int> table;
        for(int i=0;i<nums.size();i++){
            if(table.find(nums[i])!=table.end()) continue;
            table.insert(nums[i]);
            int start=i+1,end=nums.size()-1;
            while(start<end){
                if(nums[start]+nums[end]+nums[i]>0) end--;
                else if(nums[start]+nums[end]+nums[i]<0) start++;
                else {
                    std::vector<int> local_ans{nums[i],nums[start],nums[end]};   
                    ans.push_back(local_ans);
                    start++;end--;
                    while(start<end && nums[start]==nums[start-1])start++;
                    while(start<end && nums[end]==nums[end+1])end--;
                    }
                }
            }
             return ans;
        }
};
```

### 暴力解法+二分搜索

```cpp
//暴力解法+二分搜索，复杂度为O(n^2logn)，最后几个样例超时
class Solution {
private:
    std::vector<int> local_ans;
    std::vector<std::vector<int>> ans;
public:
    std::vector<std::vector<int>> threeSum(std::vector<int>& nums) {
        if (nums.size() == 0 || nums.empty()) return ans;
        std::sort(nums.begin(), nums.end());
        DFS(0, nums);
        return ans;
    }

    bool BinarySearch(int start,int end,int target,std::vector<int>& nums)
    {
        while(start<=end){
        int mid = start+(end-start)/2;
            if(target<nums[mid])
                end = mid-1;
            else if(target>nums[mid])
                start=mid+1;
            else return true;
        }
        return false;
    }

    void DFS(int start, std::vector<int>& nums) {

        if (local_ans.size() == 2) {
            int target = -(local_ans[0] + local_ans[1]);
        if (BinarySearch(start,nums.size()-1,target,nums))               {
                    local_ans.push_back(target);
                    ans.push_back(local_ans);
                    local_ans.pop_back();
                    return;
                }
                 if (local_ans.size() == 2) return;
            }
           

        std::set<int> table;
        for (int i = start; i < nums.size(); i++) {
            if (table.find(nums[i]) == table.end()) {
                table.insert(nums[i]);
                local_ans.push_back(nums[i]);
                DFS(i + 1, nums);
                local_ans.pop_back();
            }
        }
    }

   
};
```

  