#  Day25 回溯 part02

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

### 组合总和 III leetcode 216

题目：找出所有相加之和为 `n` 的 `k` 个数的组合，且满足下列条件：

- 只使用数字1到9
- 每个数字 **最多使用一次** 

返回 *所有可能的有效组合的列表* 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。

**示例 1:**

```
输入: k = 3, n = 7
输出: [[1,2,4]]
解释:
1 + 2 + 4 = 7
没有其他符合的组合了。
```

思路：经典回溯问题，排列组合问题，每次回溯递归后，集合要小一位。path.size() == k 时达到叶子节点。

```C++
class Solution {
private: 
    vector<vector<int>> res;
    vector<int> path;

    void backtracking(int k, int targetSum, int sum, int startIndex)
    {
        if(path.size() == k)
        {
            if(sum == targetSum)    res.push_back(path);
            return;
        }

        for(int i = startIndex; i <= 9; i++)
        {
            sum += i;
            path.push_back(i);
            backtracking(k, targetSum, sum, i + 1);
            sum -= i;
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        backtracking(k, n, 0, 1);
        return res;
    }
};

```

**剪枝：**

两种剪枝：

1. 当sum > tatgetSum时，没必要再递归下去。
2. i 也可剪枝， 和上一题 组合 相同。

```C++
class Solution {
private:
    vector<vector<int>> result; // 存放结果集
    vector<int> path; // 符合条件的结果
    void backtracking(int targetSum, int k, int sum, int startIndex) {
        if (sum > targetSum) { // 剪枝操作
            return; // 如果path.size() == k 但sum != targetSum 直接返回
        }
        if (path.size() == k) {
            if (sum == targetSum) result.push_back(path);
            return;
        }
        for (int i = startIndex; i <= 9 - (k - path.size()) + 1; i++) { // 剪枝
            sum += i; // 处理
            path.push_back(i); // 处理
            backtracking(targetSum, k, sum, i + 1); // 注意i+1调整startIndex
            sum -= i; // 回溯
            path.pop_back(); // 回溯
        }
    }

public:
    vector<vector<int>> combinationSum3(int k, int n) {
        result.clear(); // 可以不加
        path.clear();   // 可以不加
        backtracking(n, k, 0, 1);
        return result;
    }
};
```

### 电话号码的字母组合 leetcode 17

题目：给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/11/09/200px-telephone-keypad2svg.png)

 

**示例 1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

思路：首先将键盘进行映射。接着获取索引，递归回溯。

```C++
class Solution {
private:
    const string letterMap[10] {    //拨号盘映射
        "",
        "",
        "abc",
        "def",
        "ghi",
        "jkl",
        "mno",
        "pqrs",
        "tuv",
        "wxyz",
    };
    vector<string> res;
    string s;

    void backtracking(const string& digits, int index)
    {
        if(index == digits.size())  //index为digits中遍历到第几位
        {
            res.push_back(s);
            return;
        }
        int digitIndex = digits[index] - '0';   //获取索引对应的数字
        string letters = letterMap[digitIndex];//获取索引对应的字符串
        for(int i = 0; i < letters.size(); i++)
        {
            s.push_back(letters[i]);
            backtracking(digits, index  + 1);
            s.pop_back();
        }
    }
public:
    vector<string> letterCombinations(string digits) {
        s.clear();
        res.clear();
        if (digits.size() == 0) {
            return res;
        }
        backtracking(digits, 0);
        return res;
    }
};
```

