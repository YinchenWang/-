---
title: Day52 动态规划part13
date: 2023-9-5
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day52 动态规划part13

### 动态规划理论基础

### 最长递增子序列 leetcode 300

题目：给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

**示例 1：**

```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

**示例 2：**

```
输入：nums = [0,1,0,3,2,3]
输出：4
```

**示例 3：**

```
输入：nums = [7,7,7,7,7,7,7]
输出：1 
```

**提示：**

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`

思路：

- 确定DP数组以及下标含义: 以nums[i]w为结尾的数组的最长递增子序列的长度为dp[i] 

- 确定递推公式：if (nums[i] > nums[j]) dp[i] = max(dp[i], dp[j] + 1);

- DP数组初始化:   每一个i，对应的dp[i]（即最长递增子序列）起始大小至少都是1. 

- 确定遍历顺序:    从前往后

- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.size() <= 1)   return nums.size();
        int result = 0;
        vector<int> dp(nums.size(), 1);
        for (int i = 1; i < nums.size(); ++i)
        {
            for (int j = 0; j < i; ++j)
            {
                if (nums[i] > nums[j]) dp[i] = max(dp[i], dp[j] + 1);
            }
            if (dp[i] > result) result = dp[i]; // 取长的子序列
        }
        return result;
    }
};
```

### 最长连续递增序列 leetcode 674

题目：给定一个未经排序的整数数组，找到最长且 **连续递增的子序列**，并返回该序列的长度。

**连续递增的子序列** 可以由两个下标 `l` 和 `r`（`l < r`）确定，如果对于每个 `l <= i < r`，都有 `nums[i] < nums[i + 1]` ，那么子序列 `[nums[l], nums[l + 1], ..., nums[r - 1], nums[r]]` 就是连续递增子序列。

**示例 1：**

```
输入：nums = [1,3,5,4,7]
输出：3
解释：最长连续递增序列是 [1,3,5], 长度为3。
尽管 [1,3,5,7] 也是升序的子序列, 但它不是连续的，因为 5 和 7 在原数组里被 4 隔开。 
```

**示例 2：**

```
输入：nums = [2,2,2,2,2]
输出：1
解释：最长连续递增序列是 [2], 长度为1。 
```

**提示：**

- `1 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`

思路：

- 确定DP数组以及下标含义：以i为结尾的最长连续递增子序列的长度为dp[i]

- 确定递推公式：if(nums[i] > nums[i - 1])  dp[i] = dp[i - 1] + 1

- DP数组初始化:   dp[i] = 1

- 确定遍历顺序:    从前往后

- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        vector<int> dp(nums.size(), 1);
        int result = 1;
        for (int i = 1; i < nums.size(); ++i)
        {
            if (nums[i] > nums[i - 1])
            {
                dp[i] = dp[i - 1] + 1;
            }
            if (dp[i] > result) result = dp[i];
        }
        return result;
    }
};
```

### 最长重复子数组 leetcode 718

题目：给两个整数数组 `nums1` 和 `nums2` ，返回 *两个数组中 **公共的** 、长度最长的子数组的长度* 。

**示例 1：**

```
输入：nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
输出：3
解释：长度最长的公共子数组是 [3,2,1] 。
```

**示例 2：**

```
输入：nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]
输出：5
```

**提示：**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 100`

思路：

- 确定DP数组以及下标含义：以 i - 1 为结尾的nums1 和以 j - 1 为结尾的nums2的最长重复子数组的长度为dp[i] [j]  (不用i，j结尾是为了避免第一行初始化)

- 确定递推公式：if(nums1[i - 1] == nums2[j - 1])  dp[i] [j] = dp[i - 1] [j - 1] + 1

- DP数组初始化:   dp[i] [j] = 0

- 确定遍历顺序:    从前往后

- 打印DP数组： 用于debug

    <img src="https://code-thinking-1253855093.file.myqcloud.com/pics/2021011215282060.jpg" style="zoom:50%;" />

```C++
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp (nums1.size() + 1, vector<int>(nums2.size() + 1, 0));
        int res = 0;

        for (int i = 1; i <= nums1.size(); ++i)
        {
            for (int j = 1; j <= nums2.size(); ++j)
            {
                if (nums1[i - 1] == nums2[j - 1])
                {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }
                if (dp[i][j] > res) res = dp[i][j];
            }
        }
        return res;
    }
};
```

