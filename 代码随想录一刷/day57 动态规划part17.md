---
title: Day57 动态规划part17
date: 2023-9-8
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day57 动态规划part17

### 回文子串 leetcode 647

题目：给你一个字符串 `s` ，请你统计并返回这个字符串中 **回文子串** 的数目。

**回文字符串** 是正着读和倒过来读一样的字符串。

**子字符串** 是字符串中的由连续字符组成的一个序列。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。 

**示例 1：**

```
输入：s = "abc"
输出：3
解释：三个回文子串: "a", "b", "c"
```

**示例 2：**

```
输入：s = "aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
```

**提示：**

- `1 <= s.length <= 1000`
- `s` 由小写英文字母组成

思路1：

- 确定DP数组以及下标含义: dp[i] [j] ，[i , j]的子串是否是回文的 true / false

- 确定递推公式：

- ```C++
    if(s[i] == s[j])   
    {
      if (j - i <= 1)
      {
        dp[i][j] = true;
        result++;
      }
      else if (dp[i + 1][j - 1] == true)
      {
        dp[i][j] = true;
        result++;
      }
    }
      
    ```
    
- DP数组初始化:   dp[i] [j] = false

- 确定遍历顺序:    **从下到上，从左到右遍历，这样保证dp[i + 1][j - 1]都是经过计算的**。

- 打印DP数组： 用于debug

- <img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20210121171059951-20230310132153163.jpg" style="zoom:50%;" />

```C++
class Solution {
public:
    int countSubstrings(string s) {
        vector<vector<bool>> dp(s.size(), vector<bool>(s.size(), false));
        int result = 0;

        for (int i = s.size() - 1; i >= 0; --i)
        {
            for (int j = i; j < s.size(); ++j)
            {
                if(s[i] == s[j])   
                {
                    if (j - i <= 1)
                    {
                        dp[i][j] = true;
                        result++;
                    }
                    else if (dp[i + 1][j - 1] == true)
                    {     
                        dp[i][j] = true;
                        result++;
                    }
                }
            }
        }
        return result;
    }
};
```

思路2：求出word1与word2的最长公共子序列，然后使用word1 + word2的长度剪去两个最长公共子序列的长度即可！！！

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
       vector<vector<int>> dp(word1.size()+1, vector<int>(word2.size()+1, 0));
        for (int i=1; i<=word1.size(); i++){
            for (int j=1; j<=word2.size(); j++){
                if (word1[i-1] == word2[j-1]) dp[i][j] = dp[i-1][j-1] + 1;
                else dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
            }
        }
        return word1.size()+word2.size()-dp[word1.size()][word2.size()]*2;
    }
};

```



### 最长回文子序列 leetcode 516

题目：给你一个字符串 `s` ，找出其中最长的回文子序列，并返回该序列的长度。

子序列定义为：不改变剩余字符顺序的情况下，删除某些字符或者不删除任何字符形成的一个序列。 

**示例 1：**

```
输入：s = "bbbab"
输出：4
解释：一个可能的最长回文子序列为 "bbbb" 。
```

**示例 2：**

```
输入：s = "cbbd"
输出：2
解释：一个可能的最长回文子序列为 "bb" 。
```

**提示：**

- `1 <= s.length <= 1000`
- `s` 仅由小写英文字母组成

思路：

- 确定DP数组以及下标含义: 在[i, j]范围内最长的回文子序列的长度为dp[i] [j]。

- 确定递推公式：

- ```C++
    if (s[i] == s[j]) {	//相等长度 +2
        dp[i][j] = dp[i + 1][j - 1] + 2;
    } else {	//不相等取两个子序列中最长的
        dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
    }
    ```

- DP数组初始化:   dp[i] [i] = 1

- 确定遍历顺序:    从左往右，从上往下

- 打印DP数组： 用于debug

- <img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20210127151521432.jpg" style="zoom:50%;" />

```C++
class Solution {
public:
    int longestPalindromeSubseq(string s) {
        vector<vector<int>> dp(s.size(), vector<int>(s.size(), 0));

        for (int i = 0; i < s.size(); ++i) dp[i][i] = 1;

        for (int i = s.size() - 1; i >= 0; i--) {
            for (int j = i + 1; j < s.size(); j++) {
                if (s[i] == s[j]) {
                    dp[i][j] = dp[i + 1][j - 1] + 2;
                } else {
                    dp[i][j] = max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }
        return dp[0][s.size() - 1];
    }
};
```
