---
title: Day39 动态规划 part02
date: 2023-8-19
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day39 动态规划 part02

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

### 不同路径 leetcode 62

题目：一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

```
输入：m = 3, n = 7
输出：28
```

**示例 2：**

```
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下
```

思路：

- 确定DP数组以及下标含义： dp[i] [j] ：走到i，j有几种路径。
- 确定递推公式： dp[i] [j] = dp[i - 1] [j] + dp[i] [j - 1];
- DP数组初始化:   
    - for (int i = 0; i < m; ++i) dp[i] [0] = 1;   //初始化第一行
    - for (int j = 0; j < n; ++j) dp[0] [j] = 1;   //初始化第一列

- 确定遍历顺序:    从左向右 从上往下
- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m; ++i) dp[i][0] = 1;   //初始化第一行
        for (int j = 0; j < n; ++j) dp[0][j] = 1;   //初始化第一列

        for (int i = 1; i < m; ++i)
        {
            for (int j = 1; j < n; ++j)
            {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }

        return dp[m - 1][n - 1];
    }
};
```

### 不同路径II leetcode 63

题目：一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

![img](https://code-thinking-1253855093.file.myqcloud.com/pics/20210111204901338.png)

网格中的障碍物和空位置分别用 1 和 0 来表示。

示例 1：

![img](https://code-thinking-1253855093.file.myqcloud.com/pics/20210111204939971.png)

- 输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
- 输出：2 解释：
- 3x3 网格的正中间有一个障碍物。
- 从左上角到右下角一共有 2 条不同的路径：
    1. 向右 -> 向右 -> 向下 -> 向下
    2. 向下 -> 向下 -> 向右 -> 向右

思路：

起始位置是障碍直接return。

- 确定DP数组以及下标含义： dp[i] [j] ：走到i，j有几种路径。
- 确定递推公式：if (obs[i] [j] == 0) dp[i] [j] = dp[i - 1] [j] + dp[i] [j - 1];
- DP数组初始化:   
    - for (int i = 0; i < m && obstacleGrid[i] [0] == 0; i++) dp[i] [0] = 1;
    - for (int j = 0; j < n && obstacleGrid[0] [j] == 0; j++) dp[0] [j] = 1;
- 确定遍历顺序:    从左向右 从上往下
- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();
	if (obstacleGrid[m - 1][n - 1] == 1 || obstacleGrid[0][0] == 1) //如果在起点或终点出现了障碍，直接返回0
            return 0;
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m && obstacleGrid[i][0] == 0; i++) dp[i][0] = 1;
        for (int j = 0; j < n && obstacleGrid[0][j] == 0; j++) dp[0][j] = 1;
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 1) continue;
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
};
```

### 使用最小花费爬楼梯 leetcode 746

题目：给你一个整数数组 `cost` ，其中 `cost[i]` 是从楼梯第 `i` 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

你可以选择从下标为 `0` 或下标为 `1` 的台阶开始爬楼梯。

请你计算并返回达到楼梯顶部的最低花费。

**示例 1：**

```
输入：cost = [10,15,20]
输出：15
解释：你将从下标为 1 的台阶开始。
- 支付 15 ，向上爬两个台阶，到达楼梯顶部。
总花费为 15 。
```

**示例 2：**

```
输入：cost = [1,100,1,1,1,100,1,1,100,1]
输出：6
解释：你将从下标为 0 的台阶开始。
- 支付 1 ，向上爬两个台阶，到达下标为 2 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 4 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 6 的台阶。
- 支付 1 ，向上爬一个台阶，到达下标为 7 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 9 的台阶。
- 支付 1 ，向上爬一个台阶，到达楼梯顶部。
总花费为 6 。
```

**提示：**

- `2 <= cost.length <= 1000`
- `0 <= cost[i] <= 999`

思路：

- 确定DP数组以及下标含义： dp[i]  ：爬到i层楼梯所需要的cost为dp[i]。
- 确定递推公式：  dp[i] = (dp[i -1] + cost[i -1]) ||  (dp[i - 2] + dp[i - 2]); 要求取最小，因此 dp[i] =min （ (dp[i -1] + cost[i -1]) ， (dp[i - 2] + dp[i - 2])）
- DP数组初始化:   dp[0] = 0, dp[1] = 0, 由题可知从0或者1开始消耗都为0；
- 确定遍历顺序:    从前向后
- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        vector<int> dp(cost.size() + 1);
        dp[0] = 0;
        dp[1] = 0;
        for (int i = 2; i <= cost.size(); ++i)
        {
            dp[i] = min((cost[i - 1] + dp[i - 1]),(cost[i - 2] + dp[i - 2]));
        }
        return dp[cost.size()];
    }
};
```

