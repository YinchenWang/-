---
title: Day48 动态规划part09
date: 2023-9-3
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day48 动态规划part09

### 动态规划理论基础

**背包问题：** 

![](https://code-thinking-1253855093.file.myqcloud.com/pics/20210117171307407.png)

**多重背包：**

展开成为01背包

### 打家劫舍 leetcode 198

题目：你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。 

**示例 1：**

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 2：**

```
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

思路：

- 确定DP数组以及下标含义：考虑下标 i（包含）， 能偷到的最大的金额为dp[i]。
- 确定递推公式：对于i来说

    - 偷 i :	则dp[i - 2] + nums[i]
    - 不偷 i： 则dp[i -1]

        因此 dp[i] = max( (dp[i - 2] + nume[i]), dp[i - 1])

- DP数组初始化:   dp[0] = numi[i],     dp[1] = max(nums[0], nums[1])
- 确定遍历顺序:    从小到大
- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];
        vector<int> dp(nums.size(), 0);
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);

        for (int i = 2; i < nums.size(); ++i)
        {
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);
        }
        return dp[nums.size() - 1];
    }
};
```

### 打家劫舍II leetcode 213

题目：你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 **围成一圈** ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警** 。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **在不触动警报装置的情况下** ，今晚能够偷窃到的最高金额。

**示例 1：**

```
输入：nums = [2,3,2]
输出：3
解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
```

**示例 2：**

```
输入：nums = [1,2,3,1]
输出：4
解释：你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 3：**

```
输入：nums = [1,2,3]
输出：3
```

**提示：**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`

思路：主要处理头尾两个元素，选头去尾或者选尾去头考虑即可，其余和打家劫舍I相同

- 确定DP数组以及下标含义：考虑下标 i（包含）， 能偷到的最大的金额为dp[i]。

- 确定递推公式：对于i来说

    - 偷 i :	则dp[i - 2] + nums[i]

    - 不偷 i： 则dp[i -1]

        因此 dp[i] = max( (dp[i - 2] + nume[i]), dp[i - 1])

- DP数组初始化:   dp[0] = numi[i],     dp[1] = max(nums[0], nums[1])

- 确定遍历顺序:    从小到大

- 打印DP数组： 用于debug

```C++
int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];
        int result1 = robRange(nums, 0, nums.size() - 2); // 选头去尾
        int result2 = robRange(nums, 1, nums.size() - 1); // 选尾去头
        return max(result1, result2);
    }
    // 198.打家劫舍的逻辑
    int robRange(vector<int>& nums, int start, int end) {
        if (end == start) return nums[start];
        vector<int> dp(nums.size());
        dp[start] = nums[start];
        dp[start + 1] = max(nums[start], nums[start + 1]);
        for (int i = start + 2; i <= end; i++) {
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[end];
    }
```

### 打家劫舍III leetcode 337

题目：小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 `root` 。

除了 `root` 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 **两个直接相连的房子在同一天晚上被打劫** ，房屋将自动报警。

给定二叉树的 `root` 。返回 ***在不触动警报的情况下** ，小偷能够盗取的最高金额* 。

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)

```
输入: root = [3,2,3,null,3,null,1]
输出: 7 
解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
```

**示例 2:**

![img](https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg)

```
输入: root = [3,4,5,1,3,null,1]
输出: 9
解释: 小偷一晚能够盗取的最高金额 4 + 5 = 9
```

**提示：**



- 树的节点数在 `[1, 104]` 范围内
- `0 <= Node.val <= 104`

思路：

- 确定DP数组以及下标含义：因为递归 所以只需要一个长度为2的数组即可
    - dp[0]表示不偷当前节点的最大金额 
    - dp[1]表示偷当前节点的最大金额
- 递归终止条件

```c++
if (cur == NULL) return vector<int>{0, 0};
```

- 确定遍历顺序:    后序遍历
- 打印DP数组： 用于debug

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int rob(TreeNode* root) {
        vector<int> result = robTree(root);
        return max(result[0], result[1]);
    }
    // dp长度为2的数组，0：不偷，1：偷 递归
    vector<int> robTree(TreeNode* cur) {
        if (cur == NULL) return vector<int>{0, 0};
        vector<int> left = robTree(cur->left);
        vector<int> right = robTree(cur->right);
        // 偷cur，那么就不能偷左右节点。
        int val1 = cur->val + left[0] + right[0];
        // 不偷cur，那么可以偷也可以不偷左右节点，则取较大的情况
        int val2 = max(left[0], left[1]) + max(right[0], right[1]);
        return {val2, val1};
    }
};
```

