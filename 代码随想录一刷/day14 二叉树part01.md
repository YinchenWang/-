#  Day14 二叉树 part01

### 二叉树基础理论

#### 定义二叉树

```C++
struct TreeNode{
  int val;
  TreeNode * left;
  TreeNode * right;
  TreeNode(int x) : val(x), left(NULL), right(NULL){}
}
```



#### 二叉树种类：

**满二叉树：** 如果一颗二叉树只有度为0度结点和度为2度结点，并且度为0度结点在同一层上，则这颗二叉树为满二叉树。

<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20200806185805576.png" style="zoom:50%;" />

**完全二叉树：** 在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层（h从1开始），则该层包含 1~ 2^(h-1) 个节点。

<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20200920221638903.png" style="zoom:30%;" />

**二叉搜索树：** **二叉搜索树是一个有序树**。

- 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
- 它的左、右子树也分别为二叉排序树

<img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20200806190304693.png" style="zoom:50%;" />

**平衡二叉搜索树：** 又被称为AVL（Adelson-Velsky and Landis）树，且具有以下性质：它是一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

![](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806190511967.png)

在C++中，**map、set、multimap、multiset**的底层实现都是平衡二叉搜索树，所以它们的增删操锁时间时间复杂度为 **logn**。

#### 二叉树的存储方式：

**链式存储：** ![](https://code-thinking-1253855093.file.myqcloud.com/pics/2020092019554618.png)

**顺序存储：** ![](https://code-thinking-1253855093.file.myqcloud.com/pics/20200920200429452.png)

用数组来存储二叉树遍历：

**如果父节点的数组下标是 i，那么它的左孩子就是 i \* 2 + 1，右孩子就是 i \* 2 + 2。**

#### 二叉树遍历方式 

存在两种遍历方式，根据深度优先遍历的前中后序遍历法和根据广度优先遍历的层次遍历。

#### 前序遍历

**递归前序遍历：**

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
    void Traversal(TreeNode* cur, vector<int>& vec)
    {
        if(cur == NULL) return;
        vec.push_back(cur -> val);
        Traversal(cur -> left, vec);
        Traversal(cur -> right, vec);
    }
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        Traversal(root, res);
        return res;
    }
};
```

**迭代前序遍历：**

与递归方法不同的是，迭代法使用栈来模拟其遍历过程，首先压入根结点，然后将其弹出，接着先压入右孩子再压入左孩子，然后依次弹出，如此就得到了前序遍历的结果。

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
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while(!st.empty())
        {
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node -> val);
            if (node -> right) st.push(node -> right);
            if (node -> left) st.push(node -> left);
        }
        return result;
    }
};
```

#### 中序遍历

**递归中序遍历：**

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
    void Tarversal(TreeNode* cur, vector<int>& vec)
    {
        if(cur == NULL) return;
        Tarversal(cur -> left, vec);
        vec.push_back(cur -> val);
        Tarversal(cur -> right, vec);

    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        Tarversal(root, res);
        return res;
    }
};
```

**迭代中序遍历：**

迭代的中序遍历与前序遍历代码不可通用，因为其顺序为左中右，因此这里使用cur指针来访问左孩子直到最底层，然后再进行操作。

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
   vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        stack<TreeNode*> st;
        TreeNode* cur = root;
        while (cur != NULL || !st.empty()) {
            if (cur != NULL) { // 指针来访问节点，访问到最底层
                st.push(cur); // 将访问的节点放进栈
                cur = cur->left;                // 左
            } else {
                cur = st.top(); // 从栈里弹出的数据，就是要处理的数据（放进result数组里的数据）
                st.pop();
                result.push_back(cur->val);     // 中
                cur = cur->right;               // 右
            }
        }
        return result;
    }
};
```



#### 后序遍历

**递归后序遍历：**

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
void Tarversal(TreeNode* cur, vector<int>& vec)
    {
        if(cur == NULL) return;
        Tarversal(cur -> left, vec);
        Tarversal(cur -> right, vec);
        vec.push_back(cur -> val);

    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> res;
        Tarversal(root, res);
        return res;
    }
};
```

**迭代后序遍历：**

后序遍历顺序为左右中，将前序遍历代码更改为中右左，然后将结果数组反转一下就可（女少口阿！）

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
vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if (node->left) st.push(node->left); // 相对于前序遍历，这更改一下入栈顺序 （空节点不入栈）
            if (node->right) st.push(node->right); // 空节点不入栈
        }
        reverse(result.begin(), result.end()); // 将结果反转之后就是左右中的顺序了
        return result;
    }
};
```

