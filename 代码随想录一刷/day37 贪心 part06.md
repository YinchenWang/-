---
title: Day37 贪心 part06
date: 2023-8-17
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day37 贪心 part06

### 贪心算法理论基础

贪心的本质是选择每一阶段的局部最优，从而达到全局最优。

**贪心的使用：** 

贪心没有固定的套路，验证能不能使用贪心最好的策略是举反例，**刷题或者面试的时候，手动模拟一下感觉可以局部最优推出整体最优，而且想不到反例，那么就试一试贪心**

**贪心的一般解题步骤：**

- 将问题分解为若干个子问题。
- 找出合适的贪心策略。
- 求解每一个子问题的最优解。
- 将局部最优解堆叠成全局最优解。

### 单调递增的数字 leetcode 738

题目：当且仅当每个相邻位数上的数字 `x` 和 `y` 满足 `x <= y` 时，我们称这个整数是**单调递增**的。

给定一个整数 `n` ，返回 *小于或等于 `n` 的最大数字，且数字呈 **单调递增*** 。

**示例 1:**

```
输入: n = 10
输出: 9
```

**示例 2:**

```
输入: n = 1234
输出: 1234
```

**示例 3:**

```
输入: n = 332
输出: 299
```

**提示:**

- `0 <= n <= 109`

思路：先确定最后一位，从后往前确定。（int转换成字符串处理很妙！使用falg记录没想到）

```C++
class Solution {
public:
    int monotoneIncreasingDigits(int n) {
        string str = to_string(n);
        int flag = str.size();
        for (int i = str.size() - 1; i > 0; --i)    //从往前设置前一位小于后一位
        {
            if (str[i - 1] > str[i])
            {
                str[i - 1]--;
                flag = i;
            }
        }
        for( int j = flag; j < str.size(); ++j) //将flag往后设置我=为99 确保单增且最大
        {
            str[j] = '9';
        }

        return stoi(str);
    }
};
```

### 监控二叉树 leetcode 968 hard

题目：给定一个二叉树，我们在树的节点上安装摄像头。

节点上的每个摄影头都可以监视**其父对象、自身及其直接子对象。**

计算监控树的所有节点所需的最小摄像头数量。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/29/bst_cameras_01.png)

```
输入：[0,0,null,0,0]
输出：1
解释：如图所示，一台摄像头足以监控所有节点。
```

思路：有思路，写不出，不知道怎么隔两个节点放摄像头。题解：

树中节点有三种状态：

- 0 节点无覆盖
- 1 节点有摄像头
- 2 节点有覆盖

递归过程中树中节点有以下情况：

- 左右孩子都有覆盖 （left == 2 && right == 2）
- 左右节点至少有一个节点无覆盖， 在中间节点加摄像头,父结点 return 1。  (left == 0 || right == 0)
    - left == 0 && right == 0 左右节点无覆盖
    - left == 1 && right == 0 左节点有摄像头，右节点无覆盖
    - left == 0 && right == 1 左节点有无覆盖，右节点摄像头
    - left == 0 && right == 2 左节点无覆盖，右节点覆盖
    - left == 2 && right == 0 左节点覆盖，右节点无覆盖
- 左右节点至少有一个覆盖 (left == 1 || right == 1)， 父结点return 2
    - left == 1 && right == 2 左节点有摄像头，右节点有覆盖
    - left == 2 && right == 1 左节点有覆盖，右节点有摄像头
    - left == 1 && right == 1 左右节点都有摄像头
- 树的根结点无覆盖，此时已经遍历完了，所以需要单独给根结点加一个摄像头。

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
private:
    int result;
    int traversal(TreeNode* cur)
    {
        if (cur == NULL)    return 2;   //遇见叶子节点
        int left = traversal(cur -> left);//
        int right = traversal(cur -> right);

        if (left == 2 && right == 2)    return 0;   //左右节点都有覆盖
        if (left == 0 || right == 0)        //至少有一个节点无覆盖
        {
            result++;
            return 1;
        }     
        if (left == 1 || right ==1) return 2;   //至少一个节点有覆盖
        return -1;
    }
public:
    int minCameraCover(TreeNode* root) {
        result = 0;
        // 情况4
        if (traversal(root) == 0) { // root 无覆盖
            result++;
        }
        return result;
    }
};
```
