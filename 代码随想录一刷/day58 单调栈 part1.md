---
title: Day58 单调栈 part1
date: 2023-9-9
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day58 单调栈 part1

### 每日温度 leetcode 738

题目：给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

**示例 1:**

```
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
```

**示例 2:**

```
输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]
```

**示例 3:**

```
输入: temperatures = [30,60,90]
输出: [1,1,0]
```

**提示：**

- `1 <= temperatures.length <= 105`
- `30 <= temperatures[i] <= 100`

思路：单调栈中存储的是对应元素的下标，遍历数组判断当前元素与栈顶元素的大小。

```C++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        stack<int> st;  //单调栈，存储元素下标
        vector<int> res(temperatures.size(), 0);
        st.push(0); //压入首个元素

        for (int i = 1; i < temperatures.size(); ++i)
        {
            if (temperatures[st.top()] >= temperatures[i])  //栈顶元素 >= 当前遍历元素，压栈。  这部分可优化，因为最终i都会被压栈
            {
                st.push(i);
            }
            else        //栈顶元素 < 当前遍历元素 记录，弹出栈顶再压栈
            {
                while (!st.empty() && temperatures[st.top()] < temperatures[i])
                {
                    res[st.top()] = i - st.top();
                    st.pop();
                }
                st.push(i);
            }
        }
        return res;
    }
};
```



### 下一个更大元素I leetcode 496

题目：`nums1` 中数字 `x` 的 **下一个更大元素** 是指 `x` 在 `nums2` 中对应位置 **右侧** 的 **第一个** 比 `x` 大的元素。

给你两个 **没有重复元素** 的数组 `nums1` 和 `nums2` ，下标从 **0** 开始计数，其中`nums1` 是 `nums2` 的子集。

对于每个 `0 <= i < nums1.length` ，找出满足 `nums1[i] == nums2[j]` 的下标 `j` ，并且在 `nums2` 确定 `nums2[j]` 的 **下一个更大元素** 。如果不存在下一个更大元素，那么本次查询的答案是 `-1` 。

返回一个长度为 `nums1.length` 的数组 `ans` 作为答案，满足 `ans[i]` 是如上所述的 **下一个更大元素** 。

**示例 1：**

```
输入：nums1 = [4,1,2], nums2 = [1,3,4,2].
输出：[-1,3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 4 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
- 1 ，用加粗斜体标识，nums2 = [1,3,4,2]。下一个更大元素是 3 。
- 2 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
```

**示例 2：**

```
输入：nums1 = [2,4], nums2 = [1,2,3,4].
输出：[3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 2 ，用加粗斜体标识，nums2 = [1,2,3,4]。下一个更大元素是 3 。
- 4 ，用加粗斜体标识，nums2 = [1,2,3,4]。不存在下一个更大元素，所以答案是 -1 。
```

**提示：**

- `1 <= nums1.length <= nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 104`
- `nums1`和`nums2`中所有整数 **互不相同**
- `nums1` 中的所有整数同样出现在 `nums2` 中

 

**进阶：**你可以设计一个时间复杂度为 `O(nums1.length + nums2.length)` 的解决方案吗？

思路：一眼看出来套壳子，不过自己写还是没能A，哈哈哈。
使用hash映射很巧妙！！！ 一下子就A了

```C++
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        stack<int> st;      //单调栈 递增
        vector<int> res(nums1.size(), -1);
        if (nums1.size() == 0) return res;

        unordered_map<int, int> umap; // key:下标元素，value：下标  对nums1做hash映射，方便2中快速找到对应位置
        for (int i = 0; i < nums1.size(); i++) {
            umap[nums1[i]] = i;
        }
        st.push(0);

        for (int i = 1; i < nums2.size(); ++i)
        {
            while (!st.empty() && nums2[i] > nums2[st.top()])
            {
                if (umap.count(nums2[st.top()]) > 0)    // 看map是否存在这个元素
                {
                    int index = umap[nums2[st.top()]]; // 根据map找到nums2[st.top()] 在 nums1中的下标
                    res[index] = nums2[i];
                }
                st.pop();
            }
            st.push(i);
        }
        return res;
    }
};
```
