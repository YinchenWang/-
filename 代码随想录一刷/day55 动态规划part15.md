---
title: Day55 动态规划part15
date: 2023-9-7
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day55 动态规划part15

### 判断子序列 leetcode 392

题目：给定字符串 **s** 和 **t** ，判断 **s** 是否为 **t** 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，`"ace"`是`"abcde"`的一个子序列，而`"aec"`不是）。

**进阶：**

如果有大量输入的 S，称作 S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？

**致谢：**

特别感谢 [@pbrother ](https://leetcode.com/pbrother/)添加此问题并且创建所有测试用例。

**示例 1：**

```
输入：s = "abc", t = "ahbgdc"
输出：true
```

**示例 2：**

```
输入：s = "axc", t = "ahbgdc"
输出：false 
```

**提示：**

- `0 <= s.length <= 100`
- `0 <= t.length <= 10^4`
- 两个字符串都只由小写字符组成。

思路：和最长公共子序列一样，判断下长度返回就行了，直接秒了。

- 确定DP数组以及下标含义: [0, i-1]的序列text1 与[0,j-1]的序列text2的最长公共子序列的长度为dp[i] [j]

- 确定递推公式：

- ```C++
    if(text1[i - 1] == text2[j - 1])   
      dp[i] [j] =  dp[i - 1] [j - 1] + 1;
    else
      dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
    ```

- DP数组初始化:   dp[i] [j] = 0 //其实只要初始化第一行与第一列即可

- 确定遍历顺序:    从左往右，从上往下

- 打印DP数组： 用于debug

```C++
class Solution {
public:
    bool isSubsequence(string s, string t) {
        if (s.size() == 0)  return true;
        int target = s.size();

        vector<vector<int>> dp(s.size() + 1, vector<int>(t.size() + 1, 0));

        for (int i = 1; i <= s.size(); ++i)
        {
            for (int j = 1; j <= t.size(); ++j)
            {
                if (s[i - 1] == t[j - 1]) 
                {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                }
                else 
                {
                    dp[i][j] = dp[i][j - 1];
                }
                if (dp[i][j] == target) return true;
            }
        }
        return false;
    }
};
```

### 不同的子序列 leetcode 115

题目：给你两个字符串 `s` 和 `t` ，统计并返回在 `s` 的 **子序列** 中 `t` 出现的个数，结果需要对 109 + 7 取模。

**示例 1：**

```
输入：s = "rabbbit", t = "rabbit"
输出：3
解释：
如下所示, 有 3 种可以从 s 中得到 "rabbit" 的方案。
rabbbit
rabbbit
rabbbit
```

**示例 2：**

```
输入：s = "babgbag", t = "bag"
输出：5
解释：
如下所示, 有 5 种可以从 s 中得到 "bag" 的方案。 
babgbag
babgbag
babgbag
babgbag
babgbag 
```

**提示：**

- `1 <= s.length, t.length <= 1000`
- `s` 和 `t` 由英文字母组成

思路：

- 确定DP数组以及下标含义: 以i-1为结尾的s中有以j-1为结尾的t的个数为dp[i] [j]

- 确定递推公式：

- ```C++
    if (s[i - 1] == t[j - 1]) {
      dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
    } else {
      dp[i][j] = dp[i - 1][j];
    }
    ```
    
- DP数组初始化:   dp[i] [j] = 0 //其实只要初始化第一行与第一列即可

- 确定遍历顺序:    从左往右，从上往下

- 打印DP数组： 用于debug

- <img src="https://code-thinking.cdn.bcebos.com/pics/115.%E4%B8%8D%E5%90%8C%E7%9A%84%E5%AD%90%E5%BA%8F%E5%88%97.jpg" style="zoom:40%;" />

```C++
class Solution {
public:
    int numDistinct(string s, string t) {
        vector<vector<uint64_t>> dp(s.size() + 1, vector<uint64_t>(t.size() + 1));
        for (int i = 0; i < s.size(); ++i)  dp[i][0] = 1;
        for (int j = 1; j < t.size(); ++j)  dp[0][j] = 0;

        for (int i = 1; i <= s.size(); ++i)
        {
            for (int j = 1; j <= t.size(); ++j)
            {
                if (s[i - 1] == t[j - 1])
                {
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1][j];
                }
                else
                {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[s.size()][t.size()];
    }
};
```
