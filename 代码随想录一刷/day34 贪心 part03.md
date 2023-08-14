---
title: Day34 贪心 part03
date: 2023-8-13
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day34 贪心 part03

### 贪心算法理论基础

贪心的本质是选择每一阶段的局部最优，从而达到全局最优。

**贪心的使用：** 

贪心没有固定的套路，验证能不能使用贪心最好的策略是举反例，**刷题或者面试的时候，手动模拟一下感觉可以局部最优推出整体最优，而且想不到反例，那么就试一试贪心**

**贪心的一般解题步骤：**

- 将问题分解为若干个子问题。
- 找出合适的贪心策略。
- 求解每一个子问题的最优解。
- 将局部最优解堆叠成全局最优解。

### K次取反后最大化的数组和 leetcode 1005

题目：给你一个整数数组 `nums` 和一个整数 `k` ，按以下方法修改该数组：

- 选择某个下标 `i` 并将 `nums[i]` 替换为 `-nums[i]` 。

重复这个过程恰好 `k` 次。可以多次选择同一个下标 `i` 。

以这种方式修改数组后，返回数组 **可能的最大和** 。

**示例 1：**

```
输入：nums = [4,2,3], k = 1
输出：5
解释：选择下标 1 ，nums 变为 [4,-2,3] 。
```

思路： 思路如注释，排序取反求和即可。

```C++
class Solution {
public:
    static bool cmp(int a, int b) {
        return abs(a) > abs(b);
    }
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end(), cmp);	//按绝对值大小从大到小排序
        for (int i = 0; i < nums.size(); i++) { // 遇见负数取反
            if (nums[i] < 0 && k > 0) {
                nums[i] *= -1;
                k--;
            }
        }
        if (k % 2 == 1) nums[nums.size() - 1] *= -1; // 剩余取最小正数一直取反
        int result = 0;
        for (int a : nums) result += a;        // 求和
        return result;
    }
};
```

### 加油站 leetcode 134

题目：在一条环路上有 `n` 个加油站，其中第 `i` 个加油站有汽油 `gas[i]` 升。

你有一辆油箱容量无限的的汽车，从第 `i` 个加油站开往第 `i+1` 个加油站需要消耗汽油 `cost[i]` 升。你从其中的一个加油站出发，开始时油箱为空。

给定两个整数数组 `gas` 和 `cost` ，如果你可以按顺序绕环路行驶一周，则返回出发时加油站的编号，否则返回 `-1` 。如果存在解，则 **保证** 它是 **唯一** 的。

**示例 1:**

```
输入: gas = [1,2,3,4,5], cost = [3,4,5,1,2]
输出: 3
解释:
从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油
开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油
开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油
开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油
开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油
开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。
因此，3 可为起始索引。
```

思路：totalSum的用法很妙！计算每个加油站的净剩油耗，如果某个加油站前面的净剩油耗和无法满足当前加油站的净剩油耗，则该加油站无法到达，从下一个加油站重新计算。遍历完成后弱totalSum < 0 则代表无法走通。

```C++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int curSum = 0;
        int totalSum = 0;
        int start = 0;
        for(int i = 0; i < gas.size(); ++i)
        {
            curSum += gas[i] - cost[i];
            totalSum += gas[i] - cost[i];
            if(curSum < 0)
            {
                curSum = 0;
                start = i + 1;
            }
        }
        if (totalSum < 0)   return -1;
        return start;
    }
};
```

### 分发糖果 leetcode 135

题目：`n` 个孩子站成一排。给你一个整数数组 `ratings` 表示每个孩子的评分。

你需要按照以下要求，给这些孩子分发糖果：

- 每个孩子至少分配到 `1` 个糖果。
- 相邻两个孩子评分更高的孩子会获得更多的糖果。

请你给每个孩子分发糖果，计算并返回需要准备的 **最少糖果数目** 。

**示例 1：**

```
输入：ratings = [1,0,2]
输出：5
解释：你可以分别给第一个、第二个、第三个孩子分发 2、1、2 颗糖果。
```

思路：不要同时判断左右是否符合条件，先左后右，最后选择满足左右最大的糖果数。

```C++
class Solution {
public:
    int candy(vector<int>& ratings) {
        vector<int> nums(ratings.size(), 1);
        for(int i = 1; i < ratings.size(); ++i)
        {
            if (ratings[i] > ratings[i- 1 ])  nums[i] = nums[i - 1] + 1;
        }
        for (int i = ratings.size() - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1] ) {
                nums[i] = max(nums[i], nums[i + 1] + 1);
            }
        }
        int res = 0;
        for(int nb : nums)
        {
            res += nb;
        }
        return res;
    }
};
```

