---
title: Day42 动态规划part04
date: 2023-8-22
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day42 动态规划part04

### 动态规划理论基础

**背包问题：** 

![](https://code-thinking-1253855093.file.myqcloud.com/pics/20210117171307407.png)

**01背包：**

有n件物品和一个最多能背重量为w 的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。**每件物品只能用一次**，求解将哪些物品装入背包里物品价值总和最大。

背包最大重量为4。

物品为：

|       | 重量 | 价值 |
| ----- | ---- | ---- |
| 物品0 | 1    | 15   |
| 物品1 | 3    | 20   |
| 物品2 | 4    | 30   |

**暴力解法:**

使用暴力解法就是使用回溯法搜索出所有的情况。时间复杂度为指数级 $O(2^n)$, n为物品数量。

**二维DP数组解01背包:**

- 确定DP数组以及下标含义：d**p[i] [j] 表示从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少**。![](https://code-thinking-1253855093.file.myqcloud.com/pics/20210110103003361.png)

- 确定递推公式： 

    - **不放物品 i: ** 由dp[i -1] [j]推出，背包容量为 j， 里面不放物品 i 的最大价值，此刻dp[i] [j] 就是dp[i -1] [j].

    - **放物品 i** 由dp[i -1] [j - weight[i]]推出，dp[i -1] [j - weight[i]]为背包容量为 j - weight[i]时不放物品 i 的最大价值，所以加上物品 i 的价值就是背包放入物品 i 的价值，为 ：dp[i -1] [j - weight[i]] + value[i]。

        因此，递推公式为：

        **dp[i] [j] = max (dp[i - 1] [j] , dp[i -1] [j - weight[i]] + value[i])**

- DP数组初始化:  

    -  背包容量为0 则总和一定为0，所以：

    - ```text
        for (int j = 0 ; j < weight[0]; j++) {  // 当然这一步，如果把dp数组预先初始化为0了，这一步就可以省略，但很多同学应该没有想清楚这一点。
            dp[0][j] = 0;
        }
        ```

    - 当j < weight[0]时，dp[0] [j] = 0, 当j >= weight[0]时，dp[0] [j] = value[0].

    - ```text
        for (int j = weight[0]; j <= bagweight; j++) {
            dp[0][j] = value[0];
        }
        ```

- 确定遍历顺序:    二维情况可以颠倒, 正序倒序都可

- 打印DP数组： 用于debug

```C++
class Solution {
public:
    void problem_2d_bag () {
      vector<int> weight = {1, 3, 4};
      vector<int> value = {15, 20, 30};
      int bagWeight = 4;
      
      vector<vector<int>> dp(weight.size(), vector<int>(bagWeight + 1, 0));
      //初始化
      for (int j = weight[0]; j <= bagWeight; j++) {
        dp[0][j] = value[0];
      }
      
      for(int i = 1; i < weight.size(); i++) { // 遍历物品
        for(int j = 0; j <= bagWeight; j++) { // 遍历背包容量
            if (j < weight[i]) dp[i][j] = dp[i - 1][j];
            else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

        }
    }

    cout << dp[weight.size() - 1][bagWeight] << endl;
      
    }
};
```

**一维DP数组解01背包问题：**

在使用二维数组的时候，递推公式：dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);

**其实可以发现如果把dp[i - 1]那一层拷贝到dp[i]上，表达式完全可以是：dp[i][j] = max(dp[i][j], dp[i][j - weight[i]] + value[i]);**

**与其把dp[i - 1]这一层拷贝到dp[i]上，不如只用一个一维数组了**，只用dp[j]（一维数组，也可以理解是一个滚动数组）。

- 确定DP数组以及下标含义：dp[j]表示：容量为j的背包，所背的物品价值可以最大为dp[j]。

- 确定递推公式： dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

- DP数组初始化:  

    -  dp[0] = 0：

    - dp数组在推导的时候一定是取价值最大的数，如果题目给的价值都是正整数那么非0下标都初始化为0就可以了

    - ```text
        for(int i = 0; i < weight.size(); i++) { // 遍历物品
            for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
                dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        
            }
        }
        ```

- 确定遍历顺序:    必须逆序遍历！！**为了保证物品i只被放入一次**

- 打印DP数组： 用于debug

```C++
void test_1_wei_bag_problem() {
    vector<int> weight = {1, 3, 4};
    vector<int> value = {15, 20, 30};
    int bagWeight = 4;

    // 初始化
    vector<int> dp(bagWeight + 1, 0);
    for(int i = 0; i < weight.size(); i++) { // 遍历物品
        for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    cout << dp[bagWeight] << endl;
}

int main() {
    test_1_wei_bag_problem();
}

```



### 分割等和子集 leetcode 416

题目：给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

（背包应用类题目） 

**示例 1：**

```
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
```

**示例 2：**

```
输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集。
```

**提示：**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

思路：

- 确定DP数组以及下标含义： 容量为 j ，它的价值是dp[j]。 Dp[target] == target时，背包装满了。 本题为target =  sum / 2。
- 确定递推公式：dp[j] = max(dp[j],  dp[j - nums[i]] + nums[i])。 // 01背包
- DP数组初始化:   dp[0] = 0, 其他也为0 。
- 确定遍历顺序:    从后往前遍历
- 打印DP数组： 用于debug

```C++
class Solution {
private:
    int getTarget(vector<int>& nums)
    {
        int sum = 0;
        for( int i : nums)
        {
            sum += i;
        }
        if (sum % 2 == 1)   return -1;
        return sum / 2;
    }
public:
    bool canPartition(vector<int>& nums) {
        int target = getTarget(nums);
        if (target == -1)   return false;
        vector<int> dp(10001, 0);

        for (int i = 0; i < nums.size(); ++i)
        {
            for (int j = target; j >= nums[i]; --j)
            {
                dp[j] = max(dp[j], dp[j - nums[i]] + nums[i]);
            }
        }

        // 集合中的元素正好可以凑成总和target
        if (dp[target] == target) return true;
        return false;
    }
};
```
