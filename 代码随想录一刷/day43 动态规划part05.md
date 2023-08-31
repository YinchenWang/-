---
title: Day43 动态规划part05
date: 2023-8-23
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day43 动态规划part05

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

### 最后一块石头的重量II leetcode1049

题目：有一堆石头，用整数数组 `stones` 表示。其中 `stones[i]` 表示第 `i` 块石头的重量。

每一回合，从中选出**任意两块石头**，然后将它们一起粉碎。假设石头的重量分别为 `x` 和 `y`，且 `x <= y`。那么粉碎的可能结果如下：

- 如果 `x == y`，那么两块石头都会被完全粉碎；
- 如果 `x != y`，那么重量为 `x` 的石头将会完全粉碎，而重量为 `y` 的石头新重量为 `y-x`。

最后，**最多只会剩下一块** 石头。返回此石头 **最小的可能重量** 。如果没有石头剩下，就返回 `0`。

**示例 1：**

```
输入：stones = [2,7,4,1,8,1]
输出：1
解释：
组合 2 和 4，得到 2，所以数组转化为 [2,7,1,8,1]，
组合 7 和 8，得到 1，所以数组转化为 [2,1,1,1]，
组合 2 和 1，得到 1，所以数组转化为 [1,1,1]，
组合 1 和 1，得到 0，所以数组转化为 [1]，这就是最优值。
```

思路：

类似于分割等和数组，将重量尽量分为等重的两堆，然后砸石头。

- 确定DP数组以及下标含义：装满容量为 j 的背包的最大重量为 dp[j] 
- 确定递推公式： dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);
- DP数组初始化:   dp数组初始化为0
  
- 确定遍历顺序:    从大往小遍历
- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        if (stones.size() == 1) return stones[0];
        vector<int> dp(15001, 0);
        int sum = 0;
        for (int i : stones)
        {
            sum += i;
        }

        int target = sum / 2;

        for (int i = 0; i < stones.size(); ++i)
        {
            for (int j = target; j >= stones[i]; --j)
            {
                 dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);
            }
        }

        return sum - dp[target] - dp[target];
    }
};
```

### 目标和 leetcode 494

题目：给你一个非负整数数组 `nums` 和一个整数 `target` 。

向数组中的每个整数前添加 `'+'` 或 `'-'` ，然后串联起所有整数，可以构造一个 **表达式** ：

- 例如，`nums = [2, 1]` ，可以在 `2` 之前添加 `'+'` ，在 `1` 之前添加 `'-'` ，然后串联起来得到表达式 `"+2-1"` 。

返回可以通过上述方法构造的、运算结果等于 `target` 的不同 **表达式** 的数目。

**示例 1：**

```
输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

**示例 2：**

```
输入：nums = [1], target = 1
输出：1 
```

**提示：**

- `1 <= nums.length <= 20`
- `0 <= nums[i] <= 1000`
- `0 <= sum(nums[i]) <= 1000`
- `-1000 <= target <= 1000`

思路：

转换为左右两个序列，一个加 +号一个加-号，最后两个序列相加。如果left为正right为负，则 一定有 left - right = target  -> left - (sum - left) = target -> left = (target + sum) / 2.

- 确定DP数组以及下标含义： 装满容量为 j 的背包有dp[j]种方法
- 确定递推公式：dp[j] += dp[j - nums[i]]
- DP数组初始化:   dp[0] = 1, 非0下标初始化为0
- 确定遍历顺序:    倒序
- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int S) {
        int sum = 0;
        for (int i = 0; i < nums.size(); i++) sum += nums[i];
        if (abs(S) > sum) return 0; // 此时没有方案
        if ((S + sum) % 2 == 1) return 0; // 此时没有方案
        int bagSize = (S + sum) / 2;
        vector<int> dp(bagSize + 1, 0);
        dp[0] = 1;
        for (int i = 0; i < nums.size(); i++) {
            for (int j = bagSize; j >= nums[i]; j--) {
                dp[j] += dp[j - nums[i]];
            }
        }
        return dp[bagSize];
    }
};
```

### 一和零 leetcode 474

题目：给你一个二进制字符串数组 `strs` 和两个整数 `m` 和 `n` 。

请你找出并返回 `strs` 的最大子集的长度，该子集中 **最多** 有 `m` 个 `0` 和 `n` 个 `1` 。

如果 `x` 的所有元素也是 `y` 的元素，集合 `x` 是集合 `y` 的 **子集** 。

 

**示例 1：**

```
输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。
```

**示例 2：**

```
输入：strs = ["10", "0", "1"], m = 1, n = 1
输出：2
解释：最大的子集是 {"0", "1"} ，所以答案是 2 。 
```

**提示：**

- `1 <= strs.length <= 600`
- `1 <= strs[i].length <= 100`
- `strs[i]` 仅由 `'0'` 和 `'1'` 组成
- `1 <= m, n <= 100`

思路：

- 确定DP数组以及下标含义： **最多有i个0和j个1的strs的最大子集的大小为dp[i] [j]**
- 确定递推公式：dp[i] [j] = max(dp[i] [j], dp[i - zeroNum] [j - oneNum] + 1)
- DP数组初始化:   初始化为0
- 确定遍历顺序:    外层for循环遍历物品，内层for循环遍历背包容量且从后向前遍历！
- 打印DP数组： 用于debug

```c++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m + 1, vector<int> (n + 1, 0)); // 默认初始化0
        for (string str : strs) { // 遍历物品
            int oneNum = 0, zeroNum = 0;
            for (char c : str) {
                if (c == '0') zeroNum++;
                else oneNum++;
            }
            for (int i = m; i >= zeroNum; i--) { // 遍历背包容量且从后向前遍历！
                for (int j = n; j >= oneNum; j--) {
                    dp[i][j] = max(dp[i][j], dp[i - zeroNum][j - oneNum] + 1);
                }
            }
        }
        return dp[m][n];
    }
};
```

