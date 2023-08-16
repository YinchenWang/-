---
title: Day36 贪心 part05
date: 2023-8-16
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day36 贪心 part05

### 贪心算法理论基础

贪心的本质是选择每一阶段的局部最优，从而达到全局最优。

**贪心的使用：** 

贪心没有固定的套路，验证能不能使用贪心最好的策略是举反例，**刷题或者面试的时候，手动模拟一下感觉可以局部最优推出整体最优，而且想不到反例，那么就试一试贪心**

**贪心的一般解题步骤：**

- 将问题分解为若干个子问题。
- 找出合适的贪心策略。
- 求解每一个子问题的最优解。
- 将局部最优解堆叠成全局最优解。

### 无重叠区间 leetcode 435

题目：给定一个区间的集合 `intervals` ，其中 `intervals[i] = [starti, endi]` 。返回 *需要移除区间的最小数量，使剩余区间互不重叠* 。

**示例 1:**

```
输入: intervals = [[1,2],[2,3],[3,4],[1,3]]
输出: 1
解释: 移除 [1,3] 后，剩下的区间没有重叠。
```

思路： 让求需要删除几个区间才能不重叠，可以转换为求不交叉的区间数count，再用size（）- count即可。

```C++
class Solution {
private:
    static bool cmp(vector<int>& a, vector<int>& b)
    {
        return a[1] < b[1];
    }
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if(intervals.size() == 1)   return 0;
        sort(intervals.begin(), intervals.end(), cmp);
        int count = 1; 
        int end = intervals[0][1]; // 记录区间分割点
        for (int i = 1; i < intervals.size(); i++) {
            if (end <= intervals[i][0]) {
                end = intervals[i][1];
                count++;
            }
        }
        return intervals.size() - count;
    }
};

```

### 划分字母区间 leetcode 763

题目：给你一个字符串 `s` 。我们要把这个字符串划分为尽可能多的片段，同一字母最多出现在一个片段中。

注意，划分结果需要满足：将所有划分结果按顺序连接，得到的字符串仍然是 `s` 。

返回一个表示每个字符串片段的长度的列表。

**示例 1：**

```
输入：s = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca"、"defegde"、"hijhklij" 。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 这样的划分是错误的，因为划分的片段数较少。 
```

思路：妙蛙种子吃了妙脆角。 先遍历使用哈希法统计每个字符出现的最后位置，然后再一直遍历直到当前序列内包含的所有字符的最后出现位置处切割。

```C++
class Solution {
public:
    vector<int> partitionLabels(string s) {
        int hash[27] = {0};
        for (int i = 0; i < s.size(); ++i)
        {
            hash[s[i] - 'a'] = i;   //统计每个字符最后出现的位置
        }

        vector<int> result;
        int left = 0;
        int right = 0;

        for (int i = 0; i < s.size(); ++i)
        {
            right = max(right, hash[s[i] - 'a']);
            if (i == right) {
                result.push_back(right - left + 1);
                left = i + 1;
            }
        }
        return result;
    }
};
```

### 合并区间 leetcode 56

题目：以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 *一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间* 。

**示例 1：**

```
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

思路：和前面的一样，主要是怎么判断重叠。 lambda表达式知道但不熟悉，需要看看。

```C++
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> result;
        if (intervals.size() == 0)  return result;
        sort(intervals.begin(), intervals.end(), [](const vector<int>& a, const vector<int>& b){return a[0] < b[0];}); //lambda表达式法。

        result.push_back(intervals[0]); //将首个元素放入，后边直接判断合并
        for (int i = 1; i < intervals.size(); ++i)
        {
            if (result.back()[1] >= intervals[i][0])    //如果重叠 右区间大于左区间
            {
                 // 合并区间，只更新右边界就好，因为result.back()的左边界一定是最小值（左边界排序的）
                result.back()[1] = max(result.back()[1], intervals[i][1]);
            }
            else
            {
                result.push_back(intervals[i]); //不重叠则压入当前区间，进行下一次循环判断
            }
        }
        return result;
    }
};
```

