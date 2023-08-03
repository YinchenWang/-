#  Day18 二叉树 part05

**二叉树的深度：** 指从根节点到该节点的最长简单路径边的条数或者节点数（取决于深度从0开始还是从1开始）（通常用前序求）

**二叉树的高度：** 指从该节点到叶子节点的最长简单路径边的条数或者节点数（取决于高度从0开始还是从1开始）（通常用后序求）

### 找树左下角的值 leetcode 513

题目：给定一个二叉树的 **根节点** `root`，请找出该二叉树的 **最底层 最左边** 节点的值。

假设二叉树中至少有一个节点。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2020/12/14/tree1.jpg)

```
输入: root = [2,1,3]
输出: 1
```

**思路1：首先使用递归法来解题。由于要求的是树的左下角的值，所以三种顺序的遍历方法均可使用，找到的最大深度的第一个值就是题中要求的值。

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
    int maxDepth = INT_MIN;
    int res;
    void travsersal(TreeNode* node, int depth)
    {
        if(node -> left == NULL && node -> right == NULL)
        {
            if(depth > maxDepth)
            {
                maxDepth = depth;
                res = node -> val;
            }
            return;
        }
        //本题无需中节点的处理，只需考虑左右节点即可

        if(node -> left)
        {
            depth++;    //进入下一层，深度 +1。
            travsersal(node -> left, depth);    //递归
            depth--;     //回溯，递归返回上一层，所以深度 -1.
        }

        if(node -> right)
        {
            depth++;
            travsersal(node -> right, depth);
            depth--;
        }
    }

    int findBottomLeftValue(TreeNode* root) {
        int depth = 0;
        travsersal(root, depth);
        return res;
    }
};
```

**思路2:** 本题最简单的方法是使用层序遍历。当遍历到最底层时，最左侧节点的值就是题目要求。

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
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> que;
        if (root != NULL) que.push(root);
        int result = 0;
        while (!que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                if (i == 0) result = node->val; // 记录最后一行第一个元素
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
        }
        return result;
    }
};
```



### 路径总和 leetcode 112

题目：给你二叉树的根节点 `root` 和一个表示目标和的整数 `targetSum` 。判断该树中是否存在 **根节点到叶子节点** 的路径，这条路径上所有节点值相加等于目标和 `targetSum` 。如果存在，返回 `true` ；否则，返回 `false` 。

**叶子节点** 是指没有子节点的节点。 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum1.jpg)

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true
解释：等于目标和的根节点到叶节点路径如上图所示。
```

思路：本题需要使用遍历和回溯。通过前序遍历从根结点出发遍历到每个叶子结点。当找到符合条件的路径之后直接一层向上返回Ture，如果没找到就使用回溯继续找。注意回溯思想的应用。

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
    bool traversal(TreeNode* node, int sum)
    {
        if(node -> left == NULL && node -> right == NULL && sum == 0)   return true;
        if(node -> left == NULL && node -> right == NULL && sum != 0)   return false;

        if(node -> left)
        {
            sum -= node -> left -> val;
            if(traversal(node -> left, sum))    return true;    //满足条件则向上一层层返回true
            sum += node -> left -> val;
        }

        if(node -> right)
        {
            sum -= node -> right -> val;
            if(traversal(node -> right, sum))   return true;
            sum += node -> right -> val;
        }
        return false;
    }

    bool hasPathSum(TreeNode* root, int targetSum) {
        if (root == NULL) return false;
        return traversal(root, targetSum - root->val);
    }
};
```

### 路径总和II leetcode 113

题目：给你二叉树的根节点 `root` 和一个整数目标和 `targetSum` ，找出所有 **从根节点到叶子节点** 路径总和等于给定目标和的路径。

**叶子节点** 是指没有子节点的节点。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]
```

思路：本题和路径之和思路相同，只需要增加两个容器存储最终结果即可。

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
    void traversal(TreeNode* node, vector<int>& path, vector<vector<int>>& res, int sum)
    {
        if(node -> left == NULL && node -> right == NULL && sum == 0)
        {
            res.push_back(path);
            return;
        }

        if(node -> left == NULL && node -> right == NULL && sum != 0)   return;

        if(node -> left)
        {
            sum -= node -> left -> val;
            path.push_back(node -> left -> val);
            traversal(node -> left, path, res, sum);
            sum += node -> left -> val;
            path.pop_back();
        }

        if(node -> right)
        {
            sum -= node -> right ->val;
            path.push_back(node -> right ->val);
            traversal(node -> right, path, res, sum);
            sum += node -> right ->val;;
            path.pop_back();
        }
        return;
    }
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<int> path;
        vector<vector<int>> res;
        if(root == NULL) return res;
        path.push_back(root -> val);
        traversal(root, path, res, targetSum - root -> val);
        return res;
    }
};
```

### 从中序与后序遍历序列构造二叉树 leetcode 106

题目：给定两个整数数组 `inorder` 和 `postorder` ，其中 `inorder` 是二叉树的中序遍历， `postorder` 是同一棵树的后序遍历，请你构造并返回这颗 *二叉树* 。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
输入：inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
输出：[3,9,20,null,null,15,7]
```

思路：本题通过后序遍历与中序遍历构造二叉树。
			首先通过后序数组最后一位确定树的头节点，接着在分割中序数组得到左右两个子树的数组，再依照分割后的中序数组的大小来分割后序数组（记得删除末尾头节点） 然后使用递归即可。

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
    TreeNode* traversal(vector<int>& inorder, vector<int>& postorder)
    {
        //后序数组为空则树为空
        if(postorder.size() == 0) return NULL;
        //后序数组的最后一个值为树或者当前子树的头节点。
        int rootVal = postorder[postorder.size() - 1];
        TreeNode* root = new TreeNode(rootVal);     //构建头节点。

        //如果后序数组长度为1，则只存在头节点
        if(postorder.size() == 1) return root;

        //在中序数组中寻找切割点，即树或者子树头节点的index。
        int delimiterIndex;
        for(delimiterIndex = 0 ; delimiterIndex < inorder.size(); delimiterIndex++)
        {
            if(inorder[delimiterIndex] == rootVal) break;
        }

        //进行数组切割,中序数组切割。
        //使用左闭右开的区间[0, delimiterIndex)
        vector<int> leftInorder(inorder.begin(), inorder.begin() + delimiterIndex);
        vector<int> rightInorder(inorder.begin() + delimiterIndex + 1, inorder.end());

        //后序数组切割，[0, leftInorder.size)
        //前序数组大小与后序数组应当相同，以此为切割依据，舍弃后序数组最后一位。
        postorder.resize(postorder.size() - 1);
        vector<int> leftPostorder(postorder.begin(), postorder.begin() + leftInorder.size());
        vector<int> rightPostorder(postorder.begin() + leftInorder.size(), postorder.end());


        //进行递归
        root -> left = traversal(leftInorder, leftPostorder);
        root -> right = traversal(rightInorder, rightPostorder);

        return root;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        //不要忘了分析这两个数组的长度来剪枝
        if (inorder.size() == 0 || postorder.size() == 0) return NULL;
        return traversal(inorder, postorder);
    }
};
```

