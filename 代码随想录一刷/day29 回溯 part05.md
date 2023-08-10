---
title: Day 29 回溯 part05
date: 2023-8-9
tags:
 - algorithms
categories:
 - Algorithms
---
#  Day29 回溯 part05

### 回溯算法理论基础

回溯的本质是穷举！穷举所有可能，然后选出想要的答案

**解决的问题：** 

- 组合问题: N个数里按照一定规则找出K个数的集合
- 切割问题：一个字符串按一定规则右几种切割方式
- 子集问题：一个N个数的集合里有多少种符合条件的子集
- 排列问题：N个数按一定规则全排列，有几种排列方式
- 棋盘问题：N皇后、解数独。。。

**理解回溯：**

回溯法可抽象的理解为树形问题

因为回溯法的解决都是在集合中递归查找子集，集合的大小就构成了树的宽度，递归的深度就是树的深度。

**回溯法模版：**

```C++ 
void backtracking(参数)
{
  if (终止条件)
  {
    存放结果;
    return;
  }
  
  for(选择：本层集合中元素(书中节点孩子的数量就是集合的大小))
  {
    处理节点；
    backtracking(路径，选则列表);	//递归
    回溯，撤销处理结果；
  }
}
```

### 递增子序列 leetcode 491

题目：给你一个整数数组 `nums` ，找出并返回所有该数组中不同的递增子序列，递增子序列中 **至少有两个元素** 。你可以按 **任意顺序** 返回答案。

数组中可能含有重复元素，如出现两个整数相等，也可以视作递增序列的一种特殊情况。

**示例 1：**

```
输入：nums = [4,6,7,7]
输出：[[4,6],[4,6,7],[4,6,7,7],[4,7],[4,7,7],[6,7],[6,7,7],[7,7]]
```

思路：第一想法是排序后找（😂）不能排序，会改变顺序增加没有的结果！

浓眉大眼的竟然背叛GM！用set来去重可还行！其他的就照旧了（用数组做哈希其实更快

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> result;

    void backtracking(vector<int>& nums, int startIndex)
    {
        if(path.size() >= 2)
        {
            result.push_back(path);
        }

        unordered_set<int> uset;
        for(int i = startIndex; i < nums.size(); i++)
        {
            if ((!path.empty() && nums[i] < path.back())    //去重与非递增序列
                    || uset.find(nums[i]) != uset.end()) {
                    continue;
            }
            uset.insert(nums[i]);   //使用过的数加入set中
            path.push_back(nums[i]);
            backtracking(nums, i  + 1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> findSubsequences(vector<int>& nums) {
        backtracking(nums, 0);
        return result;
    }
};
```

### 全排列 leetcode 46

题目：给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]
```

**提示：**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有整数 **互不相同**

思路：排列问题，注意顺序不同则不同！其他很简单，使用used来记录某个数字被使用过，取数正常取就是（开始没转过弯）。

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> res;

    void backtracking(vector<int>& nums, vector<bool>& used)
    {
        if(path.size() == nums.size())
        {
            res.push_back(path);
            return;
        }

        for(int i = 0; i < nums.size(); i++)
        {
            if(used[i] == true) continue;
            used[i] = true;
            path.push_back(nums[i]);
            backtracking(nums, used);
            path.pop_back();
            used[i] = false;
        }
    }
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<bool> used(nums.size(), false);
        backtracking(nums, used);
        return res;
    }
};
```

### 全排列II leetcode 47

题目：给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列。

**示例 1：**

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**示例 2：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**提示：**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`

思路：全排列，但需要树层去重（去除重复序列） 

​			树层去重别忘了对数组进行排序！！！！！！！

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> res;

    void backtracking(vector<int>& nums, vector<bool>& used)
    {
        if(path.size() == nums.size())
        {
            res.push_back(path);
            return;
        }

        for(int i = 0; i < nums.size(); i++)
        {
            if(i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) continue;      //树层去重
            if(used[i] == false)
            {
                used[i] = true;
                path.push_back(nums[i]);
                backtracking(nums, used);
                path.pop_back();
                used[i] = false;
            }
            
        }
    }
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<bool> used(nums.size(), false);
        sort(nums.begin(), nums.end());
        backtracking(nums, used);
        return res;
    }
};
```

