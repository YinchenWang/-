#  Day17 二叉树 part04

**二叉树的深度：** 指从根节点到该节点的最长简单路径边的条数或者节点数（取决于深度从0开始还是从1开始）（通常用前序求）

**二叉树的高度：** 指从该节点到叶子节点的最长简单路径边的条数或者节点数（取决于高度从0开始还是从1开始）（通常用后序求）

### 平衡二叉树 leetcode 110

题目：给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

> 一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：true
```

**思路：**由平衡二叉树定义可得题解，在求二叉树高度的过程中对比起左右结点的高度差，如果不满足平衡二叉树则直接返回false。（在自己思索的过程中忘记给结点的子树判断了）

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int getHeight(TreeNode* node)
    {
        if(node == NULL) return 0;
        int leftHeight = getHeight(node -> left);
        if(leftHeight == -1) return -1;
        int rightHeight = getHeight(node -> right);
        if(rightHeight == -1) return -1;
        int res = abs(leftHeight - rightHeight);
        return res > 1 ? -1 : 1 + max(leftHeight, rightHeight);
    }
    bool isBalanced(TreeNode* root) {
        return getHeight(root) == -1 ? false : true;
    }
};
```

### 二叉树的所有路径 leetcode 257

题目：给你一个二叉树的根节点 `root` ，按 **任意顺序** ，返回所有从根节点到叶子节点的路径。

**叶子节点** 是指没有子节点的节点。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

```
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
```

思路：本题需要使用前序遍历和回溯。通过前序遍历可以确保从根结底出发遍历到每条路径。当找到一条路径之后使用回溯来找新的路径。注意回溯思想的应用。

```C++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void traversal(TreeNode* node, vector<int>& path, vector<string>& result)
    {
        path.push_back(node -> val);    //中，添加当前节点
        if(node -> left == NULL && node -> right == NULL)
        {
            string sPath;
            for (int i = 0; i < path.size() - 1; i++) {
                sPath += to_string(path[i]);
                sPath += "->";
            }
            sPath += to_string(path[path.size() - 1]);
            result.push_back(sPath);
            return;
        }

        if(node -> left)
        {
            traversal(node -> left, path, result);  //向左递归
            path.pop_back();        //回溯
        }
        if(node -> right)
        {
            traversal(node -> right, path, result); //向右递归
            path.pop_back();    //回溯
        }
    }
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<int> path;
        vector<string> result;
        traversal(root, path, result);
        return result;
    }
};
```

### 左叶子之和 leetcode 404

题目：给定二叉树的根节点 `root` ，返回所有左叶子之和。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/04/08/leftsum-tree.jpg)

```
输入: root = [3,9,20,null,null,15,7] 
输出: 24 
解释: 在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
```

思路：本题的重点在于左叶子的判断，只能通过父结点判断一个节点是否为左叶子。通过后序遍历将每个左叶子的值相加即可。

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        if(root == NULL) return 0;
        if (root->left == NULL && root->right== NULL) return 0;

        int leftValue = sumOfLeftLeaves(root -> left);
        if (root->left && !root->left->left && !root->left->right) { // 左子树就是一个左叶子的情况
            leftValue = root->left->val;
        }

        int rightValue = sumOfLeftLeaves(root -> right);

        int sum = leftValue + rightValue;
        return sum;
    }
};
```
