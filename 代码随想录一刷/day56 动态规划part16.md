---
title: Day56 动态规划part16
date: 2023-9-8
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day56 动态规划part16

### 两个字符串的删除操作 leetcode 538

题目：给定两个单词 `word1` 和 `word2` ，返回使得 `word1` 和 `word2` **相同**所需的**最小步数**。

**每步** 可以删除任意一个字符串中的一个字符。

**示例 1：**

```
输入: word1 = "sea", word2 = "eat"
输出: 2
解释: 第一步将 "sea" 变为 "ea" ，第二步将 "eat "变为 "ea"
```

**示例  2:**

```
输入：word1 = "leetcode", word2 = "etco"
输出：4
```

**提示：**

- `1 <= word1.length, word2.length <= 500`
- `word1` 和 `word2` 只包含小写英文字母

思路1：

- 确定DP数组以及下标含义: 以i-1为结尾的word1 与以j-1为结尾的word2使其相等的最小操作次数为dp[i] [j]

- 确定递推公式：

- ```C++
    if(word1[i - 1] == word2[j - 1])   
      dp[i] [j] =  dp[i - 1] [j - 1];
    else
      dp[i][j] = min(dp[i][j - 1] + 1, dp[i - 1][j] + 1， dp[i - 1][j - 1] + 2); //删一个和删两个
    
    //优化：
    dp[i][j] = min(dp[i][j - 1] + 1, dp[i - 1][j] + 1);
    ```
    
- DP数组初始化:   dp[0] [j] = j ， dp[i] [0] = i

    - dp[i] [0]：word2为空字符串，以i-1为结尾的字符串word1要删除多少个元素，才能和word2相同呢，很明显dp[i][0] = i。

        dp[0] [j]同理

- 确定遍历顺序:    从左往右，从上往下

- 打印DP数组： 用于debug

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.size() + 1, vector<int>(word2.size() + 1));

        for (int i = 0; i <= word1.size(); ++i)  dp[i][0] = i;
        for (int j = 0; j <= word2.size(); ++j)  dp[0][j] = j;

        for (int i = 1; i <= word1.size(); ++i)
        {
            for (int j = 1; j <= word2.size(); ++j)
            {
                if (word1[i - 1] == word2[j - 1])
                {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                else
                {
                    dp[i][j] = min(dp[i - 1][j] + 1, dp[i][j - 1] + 1);
                }
            }
        }
        return dp[word1.size()][word2.size()];
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



### 编辑距离 leetcode 72

题目：给你两个单词 `word1` 和 `word2`， *请返回将 `word1` 转换成 `word2` 所使用的最少操作数* 。

你可以对一个单词进行如下三种操作：

- 插入一个字符
- 删除一个字符
- 替换一个字符

**示例 1：**

```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

**示例 2：**

```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```

**提示：**

- `0 <= word1.length, word2.length <= 500`
- `word1` 和 `word2` 由小写英文字母组成

思路：

- 确定DP数组以及下标含义: 以i-1为结尾的word1 与以j-1]为结尾的word2将 `word1` 转换成 `word2` 所使用的最小操作次数为dp[i] [j]

- 确定递推公式：

- ```C++
    if (word1[i - 1] == word2[j - 1]) {
      dp[i][j] = dp[i - 1][j - 1];
    } else { //增删换
      dp[i][j] = min (	//删除一个元素与增加一个元素相同
        dp[i - 1][j] + 1,		//删word1中
        dp[i][j - 1] + 1,		//删word2中
        dp[i - 1][j - 1] + 1）	//替换元素
    }
    ```
    
- DP数组初始化:   

    - dp[i] [0] = i 	//word2为空串，word1操作i次成为空串

    - dp[0] [j] = j       //word1为空串，word2操作j次成为空串

- 确定遍历顺序:    从左往右，从上往下

- 打印DP数组： 用于debug

- <img src="https://code-thinking.cdn.bcebos.com/pics/115.%E4%B8%8D%E5%90%8C%E7%9A%84%E5%AD%90%E5%BA%8F%E5%88%97.jpg" style="zoom:40%;" />

```C++
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.size() + 1, vector<int>(word2.size() + 1));

        for (int i = 0; i <= word1.size(); ++i)  dp[i][0] = i;
        for (int j = 0; j <= word2.size(); ++j)  dp[0][j] = j;

        for (int i = 1; i <= word1.size(); ++i)
        {
            for (int j = 1; j <= word2.size(); ++j)
            {
                if (word1[i - 1] == word2[j - 1])
                {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                else
                {
                    dp[i][j] = min({dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]}) + 1;
                }
            }
        }
        return dp[word1.size()][word2.size()];
    }
};
```
