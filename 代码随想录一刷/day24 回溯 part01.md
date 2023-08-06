#  Day24 回溯 part01

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

### 组合 leetcode 77

题目：给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

你可以按 **任何顺序** 返回答案。

**示例 1：**

```
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

思路：经典回溯问题，排列组合问题，每次回溯递归后，集合要小一位。path.size() == k 时达到叶子节点。

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> result;

    void backtracking(int n, int k, int startIndex)
    {
        if(path.size() == k) 
        {
            result.push_back(path);
            startIndex++;
            return;
        }

        for(int i = startIndex; i <= n; i++)
        {
            path.push_back(i);
            backtracking(n, k, i + 1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combine(int n, int k) {
        backtracking(n, k, 1);
        return result;
    }
};
```

**剪枝：**

上方代码是遍历整个抽象树来查询符合条件的组合，实际上，当递归中剩余元素的个数小于 K的时候，剩下就全部不符合要求了，可以直接舍弃。优化如下：

1. 已经选择的元素个数：path.size();
2. 还需要的元素个数为: k - path.size();
3. 在集合n中至多要从该起始位置 : n - (k - path.size()) + 1，开始遍历（该数之后开始都不符合要求）

为什么有个+1呢，因为包括起始位置，我们要是一个左闭的集合。

( [x, n]的数组长度起码应该是k-path.size()才有继续搜索的可能， 那么就有 n-x+1 = k-path.size()  ， 解方程得 x = n+1 - (k-path.size()), 而且这个x是可以作为起点往下搜的 也就是for(i = s; i<=x; i++) 这里的x是可以取到的)

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> result;

    void backtracking(int n, int k, int startIndex)
    {
        if(path.size() == k) 
        {
            result.push_back(path);
            startIndex++;
            return;
        }

        for(int i = startIndex; i <= n - (k - path.size()) + 1; i++)
        {
            path.push_back(i);
            backtracking(n, k, i + 1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combine(int n, int k) {
        backtracking(n, k, 1);
        return result;
    }
};
```

