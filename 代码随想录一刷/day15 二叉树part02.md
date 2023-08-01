#  Day15 二叉树 part02

### 二叉树层序遍历

二叉树的层序遍历和图当中的广度优先搜索相同。

### 二叉树的层序遍历 leetcode 102

题目：给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[9,20],[15,7]]
```

思路：由题，使用广度优先搜索的想法，问题难点在存储其结点以及孩子。使用队列进行辅助，用size记录每一层结点的个数，每次循环size个结点，弹出其结点并且将其孩子加入队列即可。

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
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> que;
        vector<vector<int>> result;
        if(root != NULL) que.push(root);
        while(!que.empty())
        {
            int size = que.size();
            vector<int> res;
            while(size--)
            {
                TreeNode* node = que.front();
                res.push_back(node -> val);
                que.pop();
                if(node -> left) que.push(node -> left);
                if(node -> right) que.push(node -> right);
            }
            result.push_back(res);
        }
        return result;
    }
};
```

### 二叉树的层序遍历II leetcode 107

题目：给你二叉树的根节点 `root` ，返回其节点值 **自底向上的层序遍历** 。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[15,7],[9,20],[3]]
```

思路：正常使用层序遍历，最后将result数组反转一下即可。

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
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        queue<TreeNode*> que;
        vector<vector<int>> result;
        if(root != NULL) que.push(root);
        while(!que.empty())
        {
            int size = que.size();
            vector<int> res;
            while(size--)
            {
                TreeNode* node = que.front();
                res.push_back(node -> val);
                que.pop();
                if(node -> left) que.push(node -> left);
                if(node -> right) que.push(node -> right);
            }
            result.push_back(res);
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

### 二叉树的右视图 leetcode 199

题目：给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)

```
输入: [1,2,3,null,5,null,4]
输出: [1,3,4]
```

思路：层序遍历，每当遇到当前层的最右侧结点，将其存入数组即可。

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
    vector<int> rightSideView(TreeNode* root) {
        queue<TreeNode*> que;
        vector<int> result;
        if(root != NULL) que.push(root);
        while(!que.empty())
        {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                if (i == (size - 1)) result.push_back(node->val); // 将每一层的最后元素放入result数组中
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
        }
        return result;
    }
};
```

### 翻转二叉树 letcode 226

题目：给你一棵二叉树的根节点 `root` ，翻转这棵二叉树，并返回其根节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

思路：

二叉树的翻转只需要在遍历的过程中交换左右两个孩子的指针即可全部翻转。注意不要使用中序遍历，因为会翻转多次。

**递归法：**

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
    TreeNode* invertTree(TreeNode* root) {
        if (root == NULL) return root;
        swap(root->left, root->right);  // 中
        invertTree(root->left);         // 左
        invertTree(root->right);        // 右
        return root;
    }
};
```

**迭代法：**

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
    TreeNode* invertTree(TreeNode* root) {
        if(root == NULL) return root;
        std::stack<TreeNode*> st;
        st.push(root);
        while(!st.empty())
        {
            TreeNode* node = st.top();              // 中
            st.pop();
            swap(node->left, node->right);
            if(node->right) st.push(node->right);   // 右
            if(node->left) st.push(node->left);     // 左
        }
        return root;
    }
};
```

### 对称二叉树 leetcode 101

题目：给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

思路：判断一个二叉树是否对称其实就是判断根结点的两个子树是否可以相互翻转。（一开始使用层序遍历配合栈的思路是错的，没有考虑值相等但不同位置的情况）

对于两个子树能否翻转的判断要使用后序遍历！ 先判断孩子是否相等，然后判断父结点是否相等。左子树使用正常的后序遍历（左右中）右子树使用变形的后序遍历（右左中）。然后一对比即可。

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
    bool compare(TreeNode* left, TreeNode* right)
    {		//左右孩子的五种情况
        if(left == NULL && right != NULL)   return false;
        else if(left != NULL && right == NULL) return false;
        else if(left == NULL && right == NULL) return true;
        else if(left -> val != right -> val) return false;
      	else{
        //以下情况，左右结点不为空且值相等，进入递归，判断下一层
        	bool outside = compare(left -> left, right -> right);   //判断两颗子树的外侧是否一致
        	bool inside = compare(left -> right, right -> left);    //判断两颗子树的内侧是否一致
        	return outside && inside;
        }
    }
    bool isSymmetric(TreeNode* root) {
        if(root == NULL) return true;
        return compare(root -> left, root -> right);
    }
};
```

