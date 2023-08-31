---
title: Day41 动态规划part03
date: 2023-8-21
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day41 动态规划part03

### 动态规划理论基础

**什么是动态规划：** 

Dynamic programming 简称DP，如果某一问题有很多重叠的子问题，使用动态规划最有效。

动态规划中每一个状态是由上一个状态推导出来的（区别于贪心，没有状态推导）

**动态规划的解题步骤：**

- 确定DP数组以及下标含义
- 确定递推公式
- DP数组初始化
- 确定遍历顺序
- 打印DP数组

**动态规划debug：**

最好方式是将DP数组打印出来，看是否是按照思路推导的。

### 整数拆分 leetcode 343

题目：给定一个正整数 `n` ，将其拆分为 `k` 个 **正整数** 的和（ `k >= 2` ），并使这些整数的乘积最大化。

返回 *你可以获得的最大乘积* 。 

**示例 1:**

```
输入: n = 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
```

**示例 2:**

```
输入: n = 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。 
```

**提示:**

- `2 <= n <= 58`

思路：

- 确定DP数组以及下标含义：i 拆分后得到的最大乘积为 dp[i] 
- 确定递推公式： dp[i] = max(dp[i], max((i - j) * j, dp[i - j]
- DP数组初始化:   dp[2] = 1
    
- 确定遍历顺序:    从左向右 从上往下
- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int integerBreak(int n) {
        vector<int> dp(n+1);
        dp[2] = 1;
        for (int i = 3; i <= n; ++i)
        {
            for (int j = 1; j <= i / 2; j++) {
                dp[i] = max(dp[i], max((i - j) * j, dp[i - j] * j));
            }
        }
        return dp[n];
    }
};
```

### 不同的二叉搜索树 leetcode 96

题目：给你一个整数 `n` ，求恰由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的 **二叉搜索树** 有多少种？返回满足题意的二叉搜索树的种数。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

```
输入：n = 3
输出：5
```

**示例 2：**

```
输入：n = 1
输出：1
```

**提示：**

- `1 <= n <= 19`

思路：

- 确定DP数组以及下标含义： i个节点有dp[i]种二叉搜索树。
- 确定递推公式：以 j 为头节点， dp[i] += dp[j - 1] * dp[i -j]
- DP数组初始化:   dp[0] = 1, dp[1] = 1
- 确定遍历顺序:    从小到大遍历
- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1);
        dp[0] = 1;

        for (int i = 1; i <= n; ++i)
        {
            for (int j = 1; j <= i; ++j)
            {
                dp[i] += dp[j - 1] * dp[i - j];
            }
        }
        return dp[n];
    }
};
```
