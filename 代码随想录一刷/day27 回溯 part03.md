#  Day27 回溯 part03

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

### 组合总和 leetcode 39

题目：给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 所有 **不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。

`candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

 

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```

思路：本题与之前的组合总和相似，唯一区别是可重复使用元素，所以只需要更改startIndex为 i 即可

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> result;

    void backtracking(vector<int>& candidates, int target, int sum, int startIndex)
    {
        if(sum > target)    return;
        if(sum == target)  
        {
            result.push_back(path);
            return;
        }

        for(int i = startIndex; i < candidates.size(); i++)
        {
            path.push_back(candidates[i]);
            sum += candidates[i];
            backtracking(candidates, target, sum, i);
            sum -= candidates[i];
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        backtracking(candidates, target, 0, 0);
        return result;
    }
};
```

**剪枝：** 对数组先进行一个排序，这样当在一个序列中找到sum > target时，后序的就不需要再遍历了

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> result;

    void backtracking(vector<int>& candidates, int target, int sum, int startIndex)
    {
        if(sum > target)    return;
        if(sum == target)  
        {
            result.push_back(path);
            return;
        }

        for(int i = startIndex; i < candidates.size() && sum + candidates[i] <= target; i++)
        {
            path.push_back(candidates[i]);
            sum += candidates[i];
            backtracking(candidates, target, sum, i);
            sum -= candidates[i];
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0, 0);
        return result;
    }
};
```

### 组合总和II leetcode 40

题目：给定一个候选人编号的集合 `candidates` 和一个目标数 `target` ，找出 `candidates` 中所有可以使数字和为 `target` 的组合。

`candidates` 中的每个数字在每个组合中只能使用 **一次** 。

**注意：**解集不能包含重复的组合。  

**示例 1:**

```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
输出:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```

思路：本题与其他题的区别在于：

1. 最终的结果数组里不能有重复的序列。
2. candidates数组中的每个元素只能用一次。

首先需要对candidates数组进行排序，这样才能保证相同元素都是相邻的，便于剪枝。在遍历的过程中需要记录当前节点已被使用。

为了确保最终结果没有重复序列，需要对树层进行去重：当某个数值被使用过后，在后序的遍历中就跳过相同的数值，因为在第一个数进行递归的过程中已经找到了一个序列。

树层去重逻辑：**如果`candidates[i] == candidates[i - 1]` 并且 `used[i - 1] == false`，就说明：前一个树枝，使用了candidates[i - 1]，也就是说同一树层使用过candidates[i - 1]**。 

（为什么 used[i - 1] == false 就是同一树层呢，因为同一树层，used[i - 1] == false 才能表示，当前取的 candidates[i] 是从 candidates[i - 1] 回溯而来的。而 used[i - 1] == true，说明是进入下一层递归，去下一个数）

**本题同时也需要在for循环中去重，否则某个用例会TLE！！！！**

```C++
class Solution {
private:
    vector<int> path;
    vector<vector<int>> result;

    void backtracking(vector<int>& candidates, int target, int sum, int startIndex, vector<bool>& used)
    {
        if(sum == target)
        {
            result.push_back(path);
            return;
        }
        for(int i = startIndex; i < candidates.size() && sum + candidates[i] <= target; i++) // sum + candidates[i] <= target去重
        {
            if(i > 0 && candidates[i] == candidates[i - 1] && used[i-1] == false)   continue;	//树层去重。
            path.push_back(candidates[i]);
            sum += candidates[i];
            used[i] = true;
            backtracking(candidates, target, sum, i + 1, used);
            used[i] = false;
            sum -= candidates[i];
            path.pop_back();
        }
    }
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        vector<bool> used(candidates.size(), false);
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0, 0, used);
        return result;
    }
};
```

### 分割回文串 leetcode 131

题目：给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

**回文串** 是正着读和反着读都一样的字符串。

 

**示例 1：**

```
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```

**示例 2：**

```
输入：s = "a"
输出：[["a"]]
```

**提示：**

- `1 <= s.length <= 16`
- `s` 仅由小写英文字母组成

思路：本题首先需要有一个判断回文的方法，其次是进行切割。由于题目要求每个子串都满足回文，所以遍历过程中遇见不满足条件的直接进入到下一个循环。

**递归循环中如何截取子串：** 

在`for (int i = startIndex; i < s.size(); i++)`循环中，我们 定义了起始位置startIndex，那么 [startIndex, i] 就是要截取的子串。

首先判断这个子串是不是回文，如果是回文，就加入在`vector<string> path`中，path用来记录切割过的回文子串。

```C++
class Solution {
    private:
    vector<string> path;
    vector<vector<string>> result;

    bool isPalindorme(const string& s, int start, int end)
    {
        for(int i = start, j = end; i < j; i++, j--)
        {
            if(s[i] != s[j])    return false;
        }
        return true;
    }

    void backtracking(const string& s, int startIndex)
    {
        if(startIndex >= s.size())  //切割位置大于等于s长度时代表找到了一个满足要求的切割方式
        {
            result.push_back(path);
            return;
        }

        for(int i = startIndex; i < s.size(); i++)
        {
            if(isPalindorme(s, startIndex, i)) // 当前子串长度为(startIndex，i)
            {
                // 获取[startIndex,i]在s中的子串
                string str = s.substr(startIndex, i - startIndex + 1);
                path.push_back(str);
            }
            else    continue;       //不是回文串，没必要继续切割
            backtracking(s, i + 1);
            path.pop_back();
        }
    }
public:
    vector<vector<string>> partition(string s) {
        backtracking(s, 0);
        return result;
    }
};
```

