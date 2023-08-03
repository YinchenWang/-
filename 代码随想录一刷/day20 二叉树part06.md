#  Day20 二叉树 part06

**二叉树的深度：** 指从根节点到该节点的最长简单路径边的条数或者节点数（取决于深度从0开始还是从1开始）（通常用前序求）

**二叉树的高度：** 指从该节点到叶子节点的最长简单路径边的条数或者节点数（取决于高度从0开始还是从1开始）（通常用后序求）

### 最大二叉树 leetcode 654

题目：给定一个不重复的整数数组 `nums` 。 **最大二叉树** 可以用下面的算法从 `nums` 递归地构建:

1. 创建一个根节点，其值为 `nums` 中的最大值。
2. 递归地在最大值 **左边** 的 **子数组前缀上** 构建左子树。
3. 递归地在最大值 **右边** 的 **子数组后缀上** 构建右子树。

返回 *`nums` 构建的* ***最大二叉树\*** 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/12/24/tree1.jpg)

```
输入：nums = [3,2,1,6,0,5]
输出：[6,3,5,null,2,0,null,null,1]
解释：递归调用如下所示：
- [3,2,1,6,0,5] 中的最大值是 6 ，左边部分是 [3,2,1] ，右边部分是 [0,5] 。
    - [3,2,1] 中的最大值是 3 ，左边部分是 [] ，右边部分是 [2,1] 。
        - 空数组，无子节点。
        - [2,1] 中的最大值是 2 ，左边部分是 [] ，右边部分是 [1] 。
            - 空数组，无子节点。
            - 只有一个元素，所以子节点是一个值为 1 的节点。
    - [0,5] 中的最大值是 5 ，左边部分是 [0] ，右边部分是 [] 。
        - 只有一个元素，所以子节点是一个值为 0 的节点。
        - 空数组，无子节点。
```

思路：本题与前面中序后序构造二叉树一样，注意好数组的划分即可！（仔细读题！！！）

**二叉树的构造都使用前序遍历！**

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
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        if(nums.size() == 1)    return new TreeNode(nums[0]);
        int max = 0;
        int index = 0;
        for(int i = 0; i < nums.size(); i++)
        {
            if (nums[i] > max)
            {
                max = nums[i];
                index = i;
            }
        }
        TreeNode* node = new TreeNode(max);
        if(index > 0)
        {
            vector<int> left(nums.begin(), nums.begin() + index);
            node -> left = constructMaximumBinaryTree(left);
        }
        
        if(index < (nums.size() - 1))
        {
            vector<int> right(nums.begin() + index + 1, nums.end());
            node -> right = constructMaximumBinaryTree(right);
        }
        
        return node;
    }
};
```

### 合并二叉树 leetcode 617

题目：给你两棵二叉树： `root1` 和 `root2` 。

想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，**不为** null 的节点将直接作为新二叉树的节点。

返回合并后的二叉树。

**注意:** 合并过程必须从两个树的根节点开始。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/05/merge.jpg)

```
输入：root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
输出：[3,4,5,5,4,null,7]
```

思路：仔细读题，该题要求重合的节点值相加成为新节点，一方为空则不为NULL的节点直接成为新节点。直接遍历两个树，然后生成新节点即可！

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
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if(root1 == NULL) return root2;
        if(root2 == NULL) return root1;

        TreeNode* root = new TreeNode(root1 -> val + root2 -> val);

        root -> left = mergeTrees(root1 -> left, root2 -> left);
        root -> right = mergeTrees(root1 -> right, root2 -> right);

        return root;
    }
};
```

### 二叉搜索树中的搜索 leetcode 700

题目：给定二叉搜索树（BST）的根节点 `root` 和一个整数值 `val`。

你需要在 BST 中找到节点值等于 `val` 的节点。 返回以该节点为根的子树。 如果节点不存在，则返回 `null` 。

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/01/12/tree1.jpg)

```
输入：root = [4,2,7,1,3], val = 2
输出：[2,1,3]
```

二叉搜索树是一个有序树：

- 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
- 它的左、右子树也分别为二叉搜索树

思路：由于二叉搜索树的特性，所以本题实际上是让在二叉搜索树中找一个值为target的节点。当前值大于target则向左照，小于则向右找。

**递归**

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
    TreeNode* searchBST(TreeNode* root, int val) {
        if(root == NULL ||root -> val == val) return root;
        TreeNode* res = NULL;
        if(root -> val > val)
        {
            res = searchBST(root -> left, val);
        }
        if(root -> val < val)
        {
            res = searchBST(root -> right, val);
        }
        return res;
    }
};
```

**迭代**

二叉搜索树是有序的，所以直接根据情况向左右找就是了。

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
    TreeNode* searchBST(TreeNode* root, int val) {
        while(root != NULL)
        {
            if(root -> val == val) return root;
            else if(root -> val > val) root = root -> left;
            else root = root -> right;
        }
        return root;
    }
};
```



### 验证二叉搜索树 leetcode 98

题目：给你一个二叉树的根节点 `root` ，判断其是否是一个有效的二叉搜索树。

**有效** 二叉搜索树定义如下：

- 节点的左子树只包含 **小于** 当前节点的数。
- 节点的右子树只包含 **大于** 当前节点的数。
- 所有左子树和右子树自身必须也是二叉搜索树。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg)

```
输入：root = [2,1,3]
输出：true
```

思路：二叉搜索树 使用中序遍历进行验证即可。（中招了，单纯比较当前节点的左右孩子满足情况就掉陷阱了，要满足 **所有** 孩子）最简单方法中序遍历存储值到数组，判断数组是否单调递增）

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
    vector<int> res;
    void traversal(TreeNode* node)
    {
        if (node == NULL) return;

        traversal(node -> left);
        if(node)
        {
            res.push_back(node -> val);
        }
        traversal(node -> right);
    }

    bool isValidBST(TreeNode* root) {
        traversal(root);
        for(int i = 0; i < res.size() - 1; i++)
        {
            if(res[i] >= res[i + 1]) return false;
        }
        return true;
    }
};
```

