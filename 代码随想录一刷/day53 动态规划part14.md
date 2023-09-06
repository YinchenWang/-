---
title: Day53 动态规划part14
date: 2023-9-6
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day53 动态规划part14

### 最长公共子序列 leetcode 1143

题目：给定两个字符串 `text1` 和 `text2`，返回这两个字符串的最长 **公共子序列** 的长度。如果不存在 **公共子序列** ，返回 `0` 。

一个字符串的 **子序列** 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。

- 例如，`"ace"` 是 `"abcde"` 的子序列，但 `"aec"` 不是 `"abcde"` 的子序列。

两个字符串的 **公共子序列** 是这两个字符串所共同拥有的子序列。

**示例 1：**

```
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace" ，它的长度为 3 。
```

**示例 2：**

```
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc" ，它的长度为 3 。
```

**示例 3：**

```
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0 。 
```

**提示：**

- `1 <= text1.length, text2.length <= 1000`
- `text1` 和 `text2` 仅由小写英文字符组成。

思路：

- 确定DP数组以及下标含义: [0, i-1]的序列text1 与[0,j-1]的序列text2的最长公共子序列的长度为dp[i] [j]

- 确定递推公式：

- ```C++
    if(text1[i - 1] == text2[j - 1])   
      dp[i] [j] =  dp[i - 1] [j - 1] + 1;
    else
      dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
    ```

- DP数组初始化:   dp[i] [j] = 0 //其实只要初始化第一行与第一列即可

- 确定遍历顺序:    从左往右，从上往下

- 打印DP数组： 用于debug

- <img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20210210150215918.jpg" style="zoom:50%;" />

```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        vector<vector<int>> dp(text1.size() + 1, vector<int> (text2.size() + 1, 0));
        int res = 0;
        for (int i = 1; i <= text1.size(); ++i)
        {
            for (int j = 1; j <= text2.size(); ++j)
            {
                if(text1[i - 1] == text2[j - 1])   
                    dp[i] [j] =  dp[i - 1] [j - 1] + 1;
                else
                    dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
            }
        }
        return dp[text1.size()][text2.size()];
    }
};
```

### 不相交的线 leetcode 1035

题目：在两条独立的水平线上按给定的顺序写下 `nums1` 和 `nums2` 中的整数。

现在，可以绘制一些连接两个数字 `nums1[i]` 和 `nums2[j]` 的直线，这些直线需要同时满足满足：

-  `nums1[i] == nums2[j]`
- 且绘制的直线不与任何其他连线（非水平线）相交。

请注意，连线即使在端点也不能相交：每个数字只能属于一条连线。

以这种方法绘制线条，并返回可以绘制的最大连线数。

**示例 1：**

<img src="https://assets.leetcode.com/uploads/2019/04/26/142.png" alt="img" style="zoom: 25%;" />

```
输入：nums1 = [1,4,2], nums2 = [1,2,4]
输出：2
解释：可以画出两条不交叉的线，如上图所示。 
但无法画出第三条不相交的直线，因为从 nums1[1]=4 到 nums2[2]=4 的直线将与从 nums1[2]=2 到 nums2[1]=2 的直线相交。
```

**示例 2：**

```
输入：nums1 = [2,5,1,2,5], nums2 = [10,5,2,1,5,2]
输出：3
```

**示例 3：**

```
输入：nums1 = [1,3,7,1,7,5], nums2 = [1,9,2,5,1]
输出：2
```

**提示：**

- `1 <= nums1.length, nums2.length <= 500`
- `1 <= nums1[i], nums2[j] <= 2000`

思路：秒了，和上题一样求子序列

```C++
class Solution {
public:
    int maxUncrossedLines(vector<int>& nums1, vector<int>& nums2) {
        vector<vector<int>> dp(nums1.size() + 1, vector<int>(nums2.size() + 1, 0));
        for (int i = 1; i <= nums1.size(); ++i)
        {
            for (int j = 1; j <= nums2.size(); ++j)
            {
                if (nums1[i - 1] == nums2[j - 1])
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                else
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return dp[nums1.size()][nums2.size()];
    }
};
```

### 最大子数组和 leetcode 53

题目：给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

**示例 1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**示例 2：**

```
输入：nums = [1]
输出：1
```

**示例 3：**

```
输入：nums = [5,4,-1,7,8]
输出：23 
```

**提示：**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

思路：

- 确定DP数组以及下标含义：以nums[i]为结尾的最大连续子序列和为dp[i]

- 确定递推公式：dp[i] = max(dp[i - 1] + nums[i], nums[i]); //前面和小于nu ms[i]时则重新开始计算

- DP数组初始化:   dp[0] = nums[0]

- 确定遍历顺序:    从前往后

- 打印DP数组： 用于debug


```C++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
       vector<int> dp(nums.size());
       dp[0] = nums[0];
       int result= dp[0];
       for (int i = 1; i < nums.size(); ++i)
       {
           dp[i] = max(nums[i], dp[i - 1] + nums[i]);
           if (result < dp[i]) result = dp[i];
       }
       return result;
    }
};
```

