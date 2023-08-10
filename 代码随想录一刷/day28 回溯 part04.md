#  Day28 回溯 part04

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

### 复原IP地址 leetcode 93

题目：**有效 IP 地址** 正好由四个整数（每个整数位于 `0` 到 `255` 之间组成，且不能含有前导 `0`），整数之间用 `'.'` 分隔。

- 例如：`"0.1.2.201"` 和` "192.168.1.1"` 是 **有效** IP 地址，但是 `"0.011.255.245"`、`"192.168.1.312"` 和 `"192.168@1.1"` 是 **无效** IP 地址。

给定一个只包含数字的字符串 `s` ，用以表示一个 IP 地址，返回所有可能的**有效 IP 地址**，这些地址可以通过在 `s` 中插入 `'.'` 来形成。你 **不能** 重新排序或删除 `s` 中的任何数字。你可以按 **任何** 顺序返回答案。

 

**示例 1：**

```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```

思路：本题和回文串的思路类似，主要是s字符串的分割位置和合法地址的判断。

注意！在插入一个点之后，下一层递归的步长要加2.

```C++
class Solution {
private:
    vector<string> result;

    bool isValid(const string& s, int start, int end)
    {
        if(start > end) return false;
        if(s[start] == '0' && start != end)   return false;

        int num = 0;
        for(int i = start; i <= end; i++)
        {
            if(s[i] > '9' || s[i] < '0')    return false;
            num = num * 10 + (s[i] - '0');
            if (num > 255) return false; // 如果大于255了不合法
        }
        return true;
    }
    // startIndex: 搜索的起始位置，pointNum:添加逗点的数量
    void backtracking(string& s, int startIndex, int pointNum)
    {
        if(pointNum == 3)
        {
            if(isValid(s, startIndex, s.size() - 1))
            {
                result.push_back(s);
            }
            return;
        }

        for(int i = startIndex; i < s.size(); i++)
        {
            if(isValid(s, startIndex, i))   // 判断 [startIndex,i] 这个区间的子串是否合法
            {
                s.insert(s.begin() + i + 1 , '.');  // 在i的后面插入一个逗点
                pointNum++;
                backtracking(s, i + 2, pointNum);   // 插入逗点之后下一个子串的起始位置为i+2
                pointNum--;
                s.erase(s.begin() + i + 1);         // 回溯删掉逗点
            } else break; // 不合法，直接结束本层循环
        }
    }
public:
    vector<string> restoreIpAddresses(string s) {
        if (s.size() < 4 || s.size() > 12) return result; // 算是剪枝了
        backtracking(s, 0, 0);
        return result;
    }
};
```

### 子集 leetcode 78

题目：给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]] 
```

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- `nums` 中的所有元素 **互不相同**

思路：就是字符串的切割，很简单。要注意子集问题要求每次都收集结果而组合分割等问题就是在叶子结点收集（终止条件里收集）

```C++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;

    void backtracking(vector<int>& nums, int startIndex)
    {
        result.push_back(path); //每次递归都要收集结果
        if(startIndex > nums.size())    return;//可以不写，因为就是要遍历整个树

        for(int i = startIndex; i < nums.size(); i++)
        {
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        backtracking(nums, 0);
        return result;
    }
};
```

### 子集II leetcode 90

题目：给你一个整数数组 `nums` ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。返回的解集中，子集可以按 **任意顺序** 排列。

**示例 1：**

```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

**提示：**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`

思路：很简单，前面组合问题与子集结合，去重即可！

```C++
class Solution {
private:
    vector<vector<int>> result;
    vector<int> path;

    void backtracking(vector<int>& nums, int startIndex, vector<bool>& used)
    {
        result.push_back(path); //每次递归都要收集结果
        if(startIndex > nums.size())    return;//可以不写，因为就是要遍历整个树

        for(int i = startIndex; i < nums.size(); i++)
        {
            if (i > 0 && nums[i] == nums[i - 1] && used[i - 1] == false) {
                continue;
            }
            path.push_back(nums[i]);
            used[i] = true;
            backtracking(nums, i + 1, used);
            used[i] = false;
            path.pop_back();
        }
    }
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        vector<bool> used(nums.size(), false);
        sort(nums.begin(), nums.end()); // 去重需要排序
        backtracking(nums, 0, used);
        return result;
    }
};
```

