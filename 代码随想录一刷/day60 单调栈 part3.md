---
title: Day60 单调栈 part3
date: 2023-9-9
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day60 单调栈 part3

### 柱状图中最大的矩形 leetcode 84

题目：给定 *n* 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

```
输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)

```
输入： heights = [2,4]
输出： 4 
```

**提示：**

- `1 <= heights.length <=105`
- `0 <= heights[i] <= 104`

思路: 每个柱子往左右寻找比他矮的柱子来确定矩形宽度，然后来计算面积。和接雨水相反，使用单调递减栈

需要在数组首尾添加0，可以避免单增数组导致找不到结果的情况。

```C++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int result = 0;
        stack<int> st;
        // 首尾添加0，可以避免单增数组导致找不到结果的情况
        heights.insert(heights.begin(), 0); // 数组头部加入元素0
        heights.push_back(0); // 数组尾部加入元素0
        st.push(0);

        // 第一个元素已经入栈，从下标1开始
        for (int i = 1; i < heights.size(); i++) {
            while (!st.empty() && heights[i] < heights[st.top()]) { 
                    int mid = st.top();
                    st.pop();
                    if (!st.empty()) {
                        int left = st.top();
                        int right = i;
                        int w = right - left - 1;
                        int h = heights[mid];
                        result = max(result, w * h);
                    }
                }
                st.push(i);
        }
        return result;
    }
};
```

