---
title: Day51 动态规划part12
date: 2023-9-5
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day51 动态规划part12

### 动态规划理论基础

### 买卖股票的最佳时机含冷冻期 leetcode 309

题目：给定一个整数数组`prices`，其中第 `prices[i]` 表示第 `*i*` 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

- 卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

**示例 1:**

```
输入: prices = [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```

**示例 2:**

```
输入: prices = [1]
输出: 0
```

**提示：**

- `1 <= prices.length <= 5000`
- `0 <= prices[i] <= 1000`

思路：

- 确定DP数组以及下标含义：可以多次买卖，期间存在冷冻期，所以存在以下情况：

    - 目前持有股票，计算累计最大收益

    - 目前不持有股票，处于冷冻期

    - 目前不持有股票，不处于冷冻期

    
    因此dp[i] [j]代表的是在第i天中状态j所拥有的最大金额
    
- 确定递推公式：对于i来说

    - dp[i] [0] = max(dp[i - 1] [0], dp[i - 1] [2] - prices[i]);
        
    - dp[i] [1] = dp[i - 1] [0] + prices[i];
        
    -  dp[i] [2] = max(dp[i - 1] [1], dp[i - 1] [2]); 
        
    
- DP数组初始化:   dp[0] [0] = -prices[0] 

- 确定遍历顺序:    从前往后

- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        vector<vector<int>> dp(prices.size(), vector(3, 0));
        dp[0][0] -= prices[0];

        for (int i = 1; i < prices.size(); ++i)
        {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][2] - prices[i]);
            dp[i][1] = dp[i - 1][0] + prices[i];
            dp[i][2] = max(dp[i - 1][1], dp[i - 1][2]);
        }
        return max(dp[prices.size() - 1][2], dp[prices.size() - 1][1]);
    }
};
```

### 买卖股票的最佳时机含手续费 leetcode 714

题目：给定一个整数数组 `prices`，其中 `prices[i]`表示第 `i` 天的股票价格 ；整数 `fee` 代表了交易股票的手续费用。

你可以无限次地完成交易，但是你每笔交易都需要付手续费。如果你已经购买了一个股票，在卖出它之前你就不能再继续购买股票了。

返回获得利润的最大值。

**注意：**这里的一笔交易指买入持有并卖出股票的整个过程，每笔交易你只需要为支付一次手续费。

**示例 1：**

```
输入：prices = [1, 3, 2, 8, 4, 9], fee = 2
输出：8
解释：能够达到的最大利润:  
在此处买入 prices[0] = 1
在此处卖出 prices[3] = 8
在此处买入 prices[4] = 4
在此处卖出 prices[5] = 9
总利润: ((8 - 1) - 2) + ((9 - 4) - 2) = 8
```

**示例 2：**

```
输入：prices = [1,3,7,5,10,3], fee = 3
输出：6
```

**提示：**

- `1 <= prices.length <= 5 * 104`
- `1 <= prices[i] < 5 * 104`
- `0 <= fee < 5 * 104`

思路：与上题不同的时可以买卖k次，找出规律即可

- 确定DP数组以及下标含义：可以买入卖出至多两次的股票，所以在第i天中拥有以下情况：

    - 持有股票
        -  第i -1 天就持有股票，保持现状 ：dp[i - 1] [0]
        - 第i天买入股票，减去价格：dp[i -1] [1] - prices[i] 

    - 不持有股票：
        - 卖出股票：dp[i-1] [0] + prices[i] - fee
        - 第i - 1天就不持有股票,保持现状 dp[i - 1] [1]

    
    因此dp[i] [j]代表的是在第i天中状态j所拥有的最大金额
    
- 确定递推公式：对于i来说

    - dp[i] [0] = max (dp[i - 1] [0], dp[i -1] [1] - prices[i] )
    - dp[i] [1] = max (dp[i - 1] [1], dp[i-1] [0] + prices[i] - fee)

- DP数组初始化:   

- 确定遍历顺序:    从前往后

- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        vector<vector<int>> dp(prices.size(), vector<int>(2, 0));
        dp[0][0] -= prices[0];

        for (int i = 1; i < prices.size(); ++i)
        {
            dp[i][0] = max(dp[i - 1][0], dp[i - 1][1] - prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i] - fee);
        }
        return max(dp[prices.size() - 1][1], dp[prices.size() - 1][0]);
    }
};
```
