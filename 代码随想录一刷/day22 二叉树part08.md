#  Day22 二叉树 part08

**二叉树的深度：** 指从根节点到该节点的最长简单路径边的条数或者节点数（取决于深度从0开始还是从1开始）（通常用前序求）

**二叉树的高度：** 指从该节点到叶子节点的最长简单路径边的条数或者节点数（取决于高度从0开始还是从1开始）（通常用后序求）

### 二叉搜索树的最近公共祖先 leetcode 235

题目：给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

例如，给定如下二叉搜索树: root = [6,2,8,0,4,7,9,null,null,3,5]

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/binarysearchtree_improved.png)

 

**示例 1:**

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
```

思路：本题使用二叉树的最近公共祖先方法可解，不过由于本题是二叉搜索树，所以利用其特性能够更快的解决。当当前值大于两个节点的值时搜索左子树，小于时搜索右子树。其他情况则当前节点就是最近公共祖先！如此，能够快速解题。

```C++
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
        if(root == NULL)    return NULL;
        if(root -> val > p -> val && root -> val > q -> val)
        {
            TreeNode * left = lowestCommonAncestor(root -> left, p, q);
            if(left != NULL)    return left;
        }

        if(root -> val < p -> val && root -> val < q -> val)
        {
            TreeNode * right = lowestCommonAncestor(root -> right, p, q);
            if(right != NULL)   return right;
        }
        return root;
    }
};
```

### **二叉搜索树中的插入操作**  leetcode 701

题目：给定二叉搜索树（BST）的根节点 `root` 和要插入树中的值 `value` ，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 输入数据 **保证** ，新值和原始二叉搜索树中的任意节点值都不同。

**注意**，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回 **任意有效的结果** 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/05/insertbst.jpg)

```
输入：root = [4,2,7,1,3], val = 5
输出：[4,2,7,1,3,5]
解释：另一个满足题目要求可以通过的树是：
```

![](https://assets.leetcode.com/uploads/2020/10/05/bst.jpg)

思路：不用考虑那些有的没的，因为是二叉搜索树所以插入一个值的时候直接找对应子树的叶子结点插入就完事了！！！

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
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if(root == NULL)    return new TreeNode(val);
        if(root -> val > val)
        {
            root -> left = insertIntoBST(root -> left, val);
        }
        if(root -> val < val)
        {
            root -> right = insertIntoBST(root -> right, val);
        }
        return root;
    }
};
```

### 删除二叉搜索树的节点 leetcode 450

题目：给定一个二叉搜索树的根节点 **root** 和一个值 **key**，删除二叉搜索树中的 **key** 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

1. 首先找到需要删除的节点；
2. 如果找到了，删除它。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2020/09/04/del_node_1.jpg)

```
输入：root = [5,3,6,2,4,null,7], key = 3
输出：[5,4,6,2,null,null,7]
解释：给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。
一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。
另一个正确答案是 [5,2,6,null,4,null,7]。
```

![](https://assets.leetcode.com/uploads/2020/09/04/del_node_supp.jpg)

思路：删除节点有五种情况要考虑

1. 没找到 ： 返回空。
2. 删除的是叶子结点 ： 直接删除
3. 删除的结点只有左孩子 ： 将左孩子作为父结点孩子。
4. 删除的结点只有右孩子 ： 将右孩子作为父结点孩子。
5. 删除节点有两个孩子 ： 将左子树或者右子树作为父结点孩子，另一个子连接到继承的子树的最右孩子或者最左孩子。

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
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (root == nullptr) return root;
        if(root -> val == key)
        {
            if (root -> left == nullptr && root -> right == nullptr)
            {
                delete root;
                return nullptr;
            }
            else if(root -> left == nullptr)
            {
                auto retNode = root -> right;
                delete root;
                return retNode;
            }
            else if(root -> right == nullptr)
            {
                auto retNode = root -> left;
                delete root;
                return retNode;
            }
            else
            {
                TreeNode * cur = root -> right;
                while(cur -> left != nullptr)
                {
                    cur = cur -> left;
                }
                cur -> left = root -> left;
                TreeNode * tmp = root;
                root = root -> right;
                delete tmp;
                return root;
            }
        }
        if(root -> val > key)   root -> left = deleteNode(root -> left, key);
        if(root -> val < key)   root -> right = deleteNode(root -> right, key);
        return root;
    }
};
```
