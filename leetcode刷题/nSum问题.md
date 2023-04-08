
### 2sum：
情形1：元素不重复的情况
![[Pasted image 20230404133140.png]]
```C++
vector<int> twoSum(vector<int>& nums, int target) {  
    // 先对数组排序  
    sort(nums.begin(), nums.end());  
    // 左右指针  
    int lo = 0, hi = nums.size() - 1;  
    while (lo < hi) {  
        int sum = nums[lo] + nums[hi];  
        // 根据 sum 和 target 的比较，移动左右指针  
        if (sum < target) {  
            lo++;  
        } else if (sum > target) {  
            hi--;  
        } else if (sum == target) {  
            return {nums[lo], nums[hi]};  
        }  
    }  
    return {};  
}
```
情形2：元素重复的情况
![[Pasted image 20230404133238.png]]
为了避免出现多对重复的情况，应该在指针移动的时候避开哪些重复的元素：
![[Pasted image 20230404133423.png]]
从而代码应该改为：
```C++
vector<vector<int>> twoSumTarget(vector<int>& nums, int target) {  
    // nums 数组必须有序  
    sort(nums.begin(), nums.end());  
    int lo = 0, hi = nums.size() - 1;  
    vector<vector<int>> res;  
    while (lo < hi) {  
        int sum = nums[lo] + nums[hi];  
        int left = nums[lo], right = nums[hi];  
        if (sum < target) {  
            while (lo < hi && nums[lo] == left) lo++;  
        } else if (sum > target) {  
            while (lo < hi && nums[hi] == right) hi--;  
        } else {  
            res.push_back({left, right});  
            while (lo < hi && nums[lo] == left) lo++;  
            while (lo < hi && nums[hi] == right) hi--;  
        }  
    }  
    return res;  
}
```
### 3sum：
![[Pasted image 20230404133607.png]]
三数之和应该转化为1+2Sum，这个1可能是这里的每一个元素，所以应该遍历一边这些所有的元素。

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        path = []
        nums.sort()
        for i in range(len(nums)):
            target = 0 - nums[i]
            left = i + 1
            right = len(nums) - 1
            #关键部分
            if i > 0 and nums[i] == nums[i-1]:
                continue
            
            else:
                while(left<right):
                    if(nums[left]+nums[right]==target):
                        path.append([nums[i],nums[left],nums[right]])
                        #关键部分
                        while(left<right and nums[left]==nums[left+1]):
                            left=left+1
                        while(left<right and nums[right]==nums[right-1]):
                            right=right-1
                        left=left+1
                        right=right-1
                    elif(nums[right]+nums[left]>target):
                        right=right-1
                    else:
                        left=left+1
        return path
```
这里的关键是注意怎么跳过重复的元素

### nSum：
```C++
/* 注意：调用这个函数之前一定要先给 nums 排序 */  
vector<vector<int>> nSumTarget(  
    vector<int>& nums, int n, int start, int target) {  
  
    int sz = nums.size();  
    vector<vector<int>> res;  
    // 至少是 2Sum，且数组大小不应该小于 n  
    if (n < 2 || sz < n) return res;  
    // 2Sum 是 base case  
    if (n == 2) {  
        // 双指针那一套操作  
        int lo = start, hi = sz - 1;  
        while (lo < hi) {  
            int sum = nums[lo] + nums[hi];  
            int left = nums[lo], right = nums[hi];  
            if (sum < target) {  
                while (lo < hi && nums[lo] == left) lo++;  
            } else if (sum > target) {  
                while (lo < hi && nums[hi] == right) hi--;  
            } else {  
                res.push_back({left, right});  
                while (lo < hi && nums[lo] == left) lo++;  
                while (lo < hi && nums[hi] == right) hi--;  
            }  
        }  
    } else {  
        // n > 2 时，递归计算 (n-1)Sum 的结果  
        for (int i = start; i < sz; i++) {  
            vector<vector<int>>   
                sub = nSumTarget(nums, n - 1, i + 1, target - nums[i]);  
            for (vector<int>& arr : sub) {  
                // (n-1)Sum 加上 nums[i] 就是 nSum  
                arr.push_back(nums[i]);  
                res.push_back(arr);  
            }  
            while (i < sz - 1 && nums[i] == nums[i + 1]) i++;  
        }  
    }  
    return res;  
}
```
