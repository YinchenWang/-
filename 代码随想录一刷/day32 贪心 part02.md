---
title: Day32 贪心 part02
date: 2023-8-13
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day32 贪心 part02

### 贪心算法理论基础

贪心的本质是选择每一阶段的局部最优，从而达到全局最优。

**贪心的使用：** 

贪心没有固定的套路，验证能不能使用贪心最好的策略是举反例，**刷题或者面试的时候，手动模拟一下感觉可以局部最优推出整体最优，而且想不到反例，那么就试一试贪心**

**贪心的一般解题步骤：**

- 将问题分解为若干个子问题。
- 找出合适的贪心策略。
- 求解每一个子问题的最优解。
- 将局部最优解堆叠成全局最优解。

### 买卖股票的最佳时机II leetcode 122

题目：给你一个整数数组 `prices` ，其中 `prices[i]` 表示某支股票第 `i` 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以先购买，然后在 **同一天** 出售。

返回 *你能获得的 **最大** 利润* 。 

**示例 1：**

```
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6 - 3 = 3 。
     总利润为 4 + 3 = 7 。
```

思路： 将整体获利更改为每天获利即可。每当当天的获利为正时则记录获利总数即可。（收集正利润就相当于收集了低买高卖区间）

```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int left = 0;
        int right = left + 1;
        int sum = 0;
        for(left, right; right < prices.size(); ++left, ++right)
        {
            int profie = prices[right] - prices[left];
            if(profie > 0)
                sum += profie;
        }
        return sum;
    }
};
```

### 跳跃游戏 leetcode 55

题目：给你一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标，如果可以，返回 `true` ；否则，返回 `false` 。

**示例 1：**

```
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
```

**示例 2：**

```
输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
```

**提示：**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 105`

思路：主要不是怎么跳，而是当前能跳的范围！！！ 范围覆盖最后元素则Ture就完了。（取范围时要取已覆盖的值中最大的）

```C++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int cover {0};
        if(nums.size() == 1)    return true;
        for(int i = 0; i <= cover; ++i)
        {
            cover = max(i + nums[i], cover);    //取范围中最大的
            if(cover >= nums.size() - 1)    return true;
        }
        return false;
    }
};
```

### 跳跃游戏II leetcode 45

题目：给定一个长度为 `n` 的 **0 索引**整数数组 `nums`。初始位置为 `nums[0]`。

每个元素 `nums[i]` 表示从索引 `i` 向前跳转的最大长度。换句话说，如果你在 `nums[i]` 处，你可以跳转到任意 `nums[i + j]` 处:

- `0 <= j <= nums[i]` 
- `i + j < n`

返回到达 `nums[n - 1]` 的最小跳跃次数。生成的测试用例可以到达 `nums[n - 1]`。

**示例 1:**

```
输入: nums = [2,3,1,1,4]
输出: 2
解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。
```

**示例 2:**

```
输入: nums = [2,3,0,1,4]
输出: 2 
```

**提示:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 1000`
- 题目保证可以到达 `nums[n-1]`

思路：和上题类似使用覆盖范围来求解，关键在于跳跃步数的更新。每当遍历到当前范围的最大值且没有到达终点时，则代表需要再跳一步，此时更新跳跃步数即可！！

```C++
class Solution {
public:
    int jump(vector<int>& nums) {
        if(nums.size() == 1)    return 0;
        int cur {0};
        int next {0};
        int result {0};
        for(int i = 0; i < nums.size(); ++i)
        {
            next = max(i + nums[i], next);
            if(i == cur)
            {
                ++result;
                cur = next;
                if(cur >= nums.size() - 1)  break;
            }
        }
        return result;
    }
};
```

