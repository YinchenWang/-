#  Day23 二叉树 part09

**二叉树的深度：** 指从根节点到该节点的最长简单路径边的条数或者节点数（取决于深度从0开始还是从1开始）（通常用前序求）

**二叉树的高度：** 指从该节点到叶子节点的最长简单路径边的条数或者节点数（取决于高度从0开始还是从1开始）（通常用后序求）

### 修建二叉搜索树 leetcode 669

题目：给你二叉搜索树的根节点 `root` ，同时给定最小边界`low` 和最大边界 `high`。通过修剪二叉搜索树，使得所有节点的值在`[low, high]`中。修剪树 **不应该** 改变保留在树中的元素的相对结构 (即，如果没有被移除，原有的父代子代关系都应当保留)。 可以证明，存在 **唯一的答案** 。

所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/09/trim1.jpg)

```
输入：root = [1,0,2], low = 1, high = 2
输出：[1,null,2]
```

思路：本题需要注意的是，当某个节点的值小于low时，删除该节点，但该节点的右子树可能还有符合要求的节点，同理节点大于high时。此时，需要再对其对应子树进行递归修剪（不能直接返回该子树头节点就完事，因为该子树不是所有节点都绝对满足条件）。

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
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if(root == nullptr) return nullptr;
        if(root -> val < low)
        {
            return trimBST(root -> right, low, high);
        }
        if(root -> val > high)
        {
            return trimBST(root -> left, low, high);
        }
        root -> left = trimBST(root -> left, low, high);
        root -> right = trimBST(root -> right, low, high);
        return root;
    }
};
```

### 将有序数组转换为二叉搜索树 leetcode 108

题目：给你一个整数数组 `nums` ，其中元素已经按 **升序** 排列，请你将其转换为一棵 **高度平衡** 二叉搜索树。

**高度平衡** 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/18/btree1.jpg)

```
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：
```

![](https://assets.leetcode.com/uploads/2021/02/18/btree2.jpg)

思路：由于要求构造的是平衡二叉搜索树，因此默认取数组中间节点为头节点，进行构造即可。

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
    TreeNode* traversal(vector<int>& nums, int left, int right)
    {
        if(left > right)    return nullptr;
        int mid = left + ((right - left) / 2);
        TreeNode * root = new TreeNode(nums[mid]);  //取得中间节点
        root -> left = traversal(nums, left, mid - 1);
        root -> right = traversal(nums, mid + 1, right);
        return root;
    }
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return traversal(nums, 0, nums.size() - 1);
    }
};
```

把二叉搜索树转换成累加树 leetcode 538

题目：给出二叉 **搜索** 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 `node` 的新值等于原树中大于或等于 `node.val` 的值之和。

提醒一下，二叉搜索树满足下列约束条件：

- 节点的左子树仅包含键 **小于** 节点键的节点。
- 节点的右子树仅包含键 **大于** 节点键的节点。
- 左右子树也必须是二叉搜索树。

**注意：**本题和 1038: https://leetcode-cn.com/problems/binary-search-tree-to-greater-sum-tree/ 相同

**示例 1：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/05/03/tree.png)**

```
输入：[4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

思路：思路正确，使用反向的中序遍历进行累加即可。注意双指针的应用！！

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
private:
    int pre = 0;
    void traversal(TreeNode* cur)
    {
        if(cur == nullptr)  return;
        traversal(cur -> right);
        cur -> val += pre;
        pre = cur -> val;
        traversal(cur -> left);
    }
public:
    TreeNode* convertBST(TreeNode* root) {
        pre = 0;
        traversal(root);
        return root;
    }
};
```
