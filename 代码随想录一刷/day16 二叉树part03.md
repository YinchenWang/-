#  Day16 二叉树 part03

**二叉树的深度：** 指从根节点到该节点的最长简单路径边的条数或者节点数（取决于深度从0开始还是从1开始）（通常用前序求）

**二叉树的高度：** 指从该节点到叶子节点的最长简单路径边的条数或者节点数（取决于高度从0开始还是从1开始）（通常用后序求）

### 二叉树的最大深度 leetcode 104

题目：给定一个二叉树 `root` ，返回其最大深度。

二叉树的 **最大深度** 是指从根节点到最远叶子节点的最长路径上的节点数。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

 

```
输入：root = [3,9,20,null,null,15,7]
输出：3
```

**思路1：**使用层序遍历，记录每一层的深度，遍历到最后即可。（有些复杂）

```C++
class solution {
public:
    int maxDepth(TreeNode* root) {
        if (root == NULL) return 0;
        int depth = 0;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty()) {
            int size = que.size();
            depth++; // 记录深度
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
        }
        return depth;
    }
};

```

**思路2:** 由于二叉树根结点的高度就是二叉树的最大深度，所以使用后序遍历来求根结点的最大高度即可得出最大深度。

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
    int getDepth(TreeNode* node)
    {
        if(node == NULL) return 0;
        int leftDepth = getDepth(node -> left);
        int rightDepth = getDepth(node -> right);
        int depth = 1 + max(leftDepth, rightDepth);
        return depth;
    }
    int maxDepth(TreeNode* root) {
        return getDepth(root);   
    }
};
```

### 二叉树的最小深度 leetcode 111

题目：给定一个二叉树，找出其最小深度。

最小深度是**从根节点到最近叶子节点**的最短路径上的**节点数量**。

**说明：**叶子节点是指没有子节点的节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：2
```

思路：思路与求最大深度相同，不过是遍历过程中取每个的最小深度即可。要**特别注意** 最小深度是到叶子结点的结点数量，所以注意去除只有一个子树的情况！！！

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
        if(node == NULL)    return 0;   //叶子结点的高度为0；
        int leftHeight = getHeight(node -> left);   //左子树高度
        int rightHeight = getHeight(node -> right); //右子树高度

        if(node -> left == NULL && node -> right != NULL)   //左子树为空，不是叶子结点
            return 1 + rightHeight;
        if(node -> left != NULL && node -> right == NULL)   //右子树为空，不是叶子结点
            return 1 + leftHeight;

        int res = 1 + min(leftHeight, rightHeight);
        return res;
    }
    int minDepth(TreeNode* root) {
        return getHeight(root);
    }
};
```

### 完全二叉树的节点个数 leetcode 222

题目：给你一棵 **完全二叉树** 的根节点 `root` ，求出该树的节点个数。

[完全二叉树](https://baike.baidu.com/item/完全二叉树/7773232?fr=aladdin) 的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 `h` 层，则该层包含 `1~ 2h` 个节点。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/14/complete.jpg)

```
输入：root = [1,2,3,4,5,6]
输出：6
```

思路1：普通解法使用递归或者迭代进行遍历，记录数量即可。以下使用递归的后序遍历

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
    int getNum(TreeNode* node)
    {
        if(node == NULL) return 0;
        int leftNum = getNum(node -> left);
        int rightNum = getNum(node -> right);
        int num = 1 + leftNum + rightNum;
        return num;
    }
    int countNodes(TreeNode* root) {
        return getNum(root);
    }
};
```

思路2：根据完全二叉树的特性求解。完全二叉树只有两种情况，满二叉树和最后一层叶子没有满。对于满二叉树可以直接使用2^n -1 来求结点数量。

判断完全二叉树是否为满二叉树直接遍历左右子树，如果值相等则为满二叉树否则不是（完全二叉树不存在中间结点为空的情况）

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

    int countNodes(TreeNode* root) {
        if (root == nullptr) return 0;
        TreeNode* left = root->left;
        TreeNode* right = root->right;
        int leftDepth = 0, rightDepth = 0; // 这里初始为0是有目的的，为了下面求指数方便
        while (left) {  // 求左子树深度
            left = left->left;
            leftDepth++;
        }
        while (right) { // 求右子树深度
            right = right->right;
            rightDepth++;
        }
        if (leftDepth == rightDepth) {
            return (2 << leftDepth) - 1; // 注意(2<<1) 相当于2^2，所以leftDepth初始为0
        }
        return countNodes(root->left) + countNodes(root->right) + 1;
    }
};
```

