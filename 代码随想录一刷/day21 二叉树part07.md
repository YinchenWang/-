#  Day21 二叉树 part07

**二叉树的深度：** 指从根节点到该节点的最长简单路径边的条数或者节点数（取决于深度从0开始还是从1开始）（通常用前序求）

**二叉树的高度：** 指从该节点到叶子节点的最长简单路径边的条数或者节点数（取决于高度从0开始还是从1开始）（通常用后序求）

### 二叉搜索树的最小绝对差 leetcode 530

题目：给你一个二叉搜索树的根节点 `root` ，返回 **树中任意两不同节点值之间的最小差值** 。

差值是一个正数，其数值等于两值之差的绝对值。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/05/bst1.jpg)

```
输入：root = [4,2,6,1,3]
输出：1
```

思路：本题最直观的想法是将树转换为有序数组再进行比较。（可以但开销大）

使用双指针，在中序遍历中寻找差值即可。

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
private:
    int res = INT_MAX;
    TreeNode* pre = NULL;
public:
    void traversal(TreeNode* cur)
    {
        if(cur == NULL) return;
        traversal(cur -> left);
        if(pre != NULL)
        {
            res = min(res, cur -> val - pre -> val);
        }
        pre = cur;
        traversal(cur -> right);
    }
    int getMinimumDifference(TreeNode* root) {
        traversal(root);
        return res;
    }
};
```

### 二叉树中的众数 leetcode 501

题目：给你一个含重复值的二叉搜索树（BST）的根节点 `root` ，找出并返回 BST 中的所有 [众数](https://baike.baidu.com/item/众数/44796)（即，出现频率最高的元素）。

如果树中有不止一个众数，可以按 **任意顺序** 返回。

假定 BST 满足如下定义：

- 结点左子树中所含节点的值 **小于等于** 当前节点的值
- 结点右子树中所含节点的值 **大于等于** 当前节点的值
- 左子树和右子树都是二叉搜索树

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/11/mode-tree.jpg)

```
输入：root = [1,null,2,2]
输出：[2]
```

思路：第一想法遍历数，再使用哈希法求众数。

由于这是一个二叉搜索树，有序，所以可以使用双指针通过一次遍历就得到所有众数。使用中序遍历，每次统计到count > maxCount时，将结果集清空然后加入新的数。如果找到count == maxCount则将该数加入结果集。

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
private: 
    TreeNode* pre;
    int count {0};
    int maxCount {0};
    vector<int> res;
public:
    void traversal(TreeNode* cur)
    {
        if(cur == NULL) return;
        traversal(cur -> left);
        if(pre == NULL) count = 1;
        else if(pre -> val == cur -> val)    count++;
        else count = 1;
        pre = cur;

        if(count == maxCount)   res.push_back(cur -> val);
        if(count > maxCount)
        {
            maxCount = count;
            res.clear();
            res.push_back(cur -> val);
        }
        traversal(cur -> right);
    }
    vector<int> findMode(TreeNode* root) {
        traversal(root);
        return res;
    }
};
```

### 二叉树的最近公共祖先 leetcode 236

题目：给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

思路：要查找公共祖先，所以自底向上查找，使用后序遍历。某个节点为p,q的公共祖先有两种情况：

1. p, q分别为该节点左右子树上的节点
2. p, q 为父子关系

当遍历过程中遇到p或q时将其返回即可。

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == NULL) return NULL;
        if(root == p || root == q) return root;
        TreeNode * left = lowestCommonAncestor(root -> left, p, q);
        TreeNode * right = lowestCommonAncestor(root -> right, p, q);

        if(left != NULL && right != NULL) return root;	//p q为root的左右子树的节点
        if(left == NULL && right != NULL) return right; //p q都在root的右孩子的子树上且为父子关系
        if(left != NULL && right == NULL) return left; //p q都在root的左孩子的子树上且为父子关系

        return NULL;
    }
};
```
