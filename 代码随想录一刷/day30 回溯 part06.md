---
title: Day30 回溯 part06
date: 2023-8-10
tags:
 - algorithms
 - 代码随想录
categories:
 - Algorithms
---
#  Day30 回溯 part06

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

### 重新安排行程 leetcode 332

题目：给你一份航线列表 `tickets` ，其中 `tickets[i] = [fromi, toi]` 表示飞机出发和降落的机场地点。请你对该行程进行重新规划排序。

所有这些机票都属于一个从 `JFK`（肯尼迪国际机场）出发的先生，所以该行程必须从 `JFK` 开始。如果存在多种有效的行程，请你按字典排序返回最小的行程组合。

- 例如，行程 `["JFK", "LGA"]` 与 `["JFK", "LGB"]` 相比就更小，排序更靠前。

假定所有机票至少存在一种合理的行程。且所有的机票 必须都用一次 且 只能用一次。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/14/itinerary1-graph.jpg)

```
输入：tickets = [["MUC","LHR"],["JFK","MUC"],["SFO","SJC"],["LHR","SFO"]]
输出：["JFK","MUC","LHR","SFO","SJC"]
```

思路： 挺难，思路大概了解了 写不出来 放弃了

### N皇后 leetcode 51

题目：按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。

**n 皇后问题** 研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回所有不同的 **n 皇后问题** 的解决方案。

每一种解法包含一个不同的 **n 皇后问题** 的棋子放置方案，该方案中 `'Q'` 和 `'.'` 分别代表了皇后和空位。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
输入：n = 4
输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
解释：如上图所示，4 皇后问题存在两个不同的解法。
```

思路：相对来说简单些，很容易理解，在每次放棋子之后进行判断就行了。

```C++
class Solution {
private:
    vector<vector<string>> result;

    bool isValid(int row, int col, vector<string> chessboard, int n)
    {
        // 检查列
        for (int i = 0; i < row; i++) 
        { // 这是一个剪枝
            if (chessboard[i][col] == 'Q')  return false;
        }
        // 检查 45度角是否有皇后
        for (int i = row - 1, j = col - 1; i >=0 && j >= 0; i--, j--) 
        {
            if (chessboard[i][j] == 'Q')    return false;
        }
    // 检查 135度角是否有皇后
        for(int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) 
        {
            if (chessboard[i][j] == 'Q')    return false;
        }
        return true;
    }

    void backtracking(int n, int row, vector<string>& chessboard)
    {
        if(row == n)
        {
            result.push_back(chessboard);
            return;
        }

        for(int col = 0; col < n ; col++)
        {
            if(isValid(row, col, chessboard, n))
            {
                chessboard[row][col] = 'Q';
                backtracking(n, row + 1, chessboard);
                chessboard[row][col] = '.';
            }
        }
    }
public:
    vector<vector<string>> solveNQueens(int n) {
        std::vector<std::string> chessboard(n, std::string(n, '.'));
        backtracking(n, 0, chessboard);
        return result;
    }
};
```

### 解数独 leetcode 37

题目：编写一个程序，通过填充空格来解决数独问题。

数独的解法需 **遵循如下规则**：

1. 数字 `1-9` 在每一行只能出现一次。
2. 数字 `1-9` 在每一列只能出现一次。
3. 数字 `1-9` 在每一个以粗实线分隔的 `3x3` 宫内只能出现一次。（请参考示例图）

数独部分空格内已填入了数字，空白格用 `'.'` 表示。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/04/12/250px-sudoku-by-l2g-20050714svg.png)

```
输入：board = [["5","3",".",".","7",".",".",".","."],["6",".",".","1","9","5",".",".","."],[".","9","8",".",".",".",".","6","."],["8",".",".",".","6",".",".",".","3"],["4",".",".","8",".","3",".",".","1"],["7",".",".",".","2",".",".",".","6"],[".","6",".",".",".",".","2","8","."],[".",".",".","4","1","9",".",".","5"],[".",".",".",".","8",".",".","7","9"]]
输出：[["5","3","4","6","7","8","9","1","2"],["6","7","2","1","9","5","3","4","8"],["1","9","8","3","4","2","5","6","7"],["8","5","9","7","6","1","4","2","3"],["4","2","6","8","5","3","7","9","1"],["7","1","3","9","2","4","8","5","6"],["9","6","1","5","3","7","2","8","4"],["2","8","7","4","1","9","6","3","5"],["3","4","5","2","8","6","1","7","9"]]
解释：输入的数独如上图所示，唯一有效的解决方案如下所示：
```

思路：主要是棋盘检验与二维递归。 自己写的TLE了 笑死

```C++
class Solution {
private:
bool backtracking(vector<vector<char>>& board) {
    for (int i = 0; i < board.size(); i++) {        // 遍历行
        for (int j = 0; j < board[0].size(); j++) { // 遍历列
            if (board[i][j] == '.') {
                for (char k = '1'; k <= '9'; k++) {     // (i, j) 这个位置放k是否合适
                    if (isValid(i, j, k, board)) {
                        board[i][j] = k;                // 放置k
                        if (backtracking(board)) return true; // 如果找到合适一组立刻返回
                        board[i][j] = '.';              // 回溯，撤销k
                    }
                }
                return false;  // 9个数都试完了，都不行，那么就返回false
            }
        }
    }
    return true; // 遍历完没有返回false，说明找到了合适棋盘位置了
}
bool isValid(int row, int col, char val, vector<vector<char>>& board) {
    
    for (int j = 0; j < 9; j++) { // 判断列里是否重复
        if (board[j][col] == val) {
            return false;
        }
    }

    for (int i = 0; i < 9; i++) { // 判断行里是否重复
        if (board[row][i] == val) {
            return false;
        }
    }
    int startRow = (row / 3) * 3;
    int startCol = (col / 3) * 3;
    for (int i = startRow; i < startRow + 3; i++) { // 判断9方格里是否重复
        for (int j = startCol; j < startCol + 3; j++) {
            if (board[i][j] == val ) {
                return false;
            }
        }
    }
    return true;
}
public:
    void solveSudoku(vector<vector<char>>& board) {
        backtracking(board);
    }
};


```

