#  day04 链表part02

### 两两交换链表中的节点 leetcode 24

题目：给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

示例 1：


输入：head = [1,2,3,4]
输出：[2,1,4,3]

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/swap-nodes-in-pairs
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

#### 虚拟头节点法

使用虚拟头节点避免讨论头节点。 cur指向要交换的两个节点的前一个节点，交换两个节点之后将cur向右移动两位即可。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead -> next = head;
        ListNode* cur = dummyHead;

        while (cur->next != nullptr && cur -> next -> next != nullptr)
        {
            ListNode* tmp = cur -> next;
            ListNode* tmp1 = cur -> next -> next -> next;

            cur->next = cur->next->next;    // 步骤一
            cur->next->next = tmp;          // 步骤二
            cur->next->next->next = tmp1;   // 步骤三

            cur = cur->next->next;
        }
        return dummyHead -> next;
    }
};
```

### 删除链表的倒数第n个节点 leetcode 19

题目：给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

 

示例 1：


输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/remove-nth-node-from-end-of-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

使用快慢双指针，快指针先走n+1.然后快慢指针同时向后遍历到快指针指向链表尾部。此时慢指针指向的就是需要删除的节点的前一个节点。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead -> next = head;
        ListNode* fast = dummyHead;
        ListNode* slow = dummyHead;

        while (n-- && fast != nullptr)      //fast先走n步
        {
            fast = fast -> next;
        }
        fast = fast -> next; // 这一步是为了fast与solw同时移动时，slow能停在要删除节点的上一个节点。

        while (fast != nullptr)     //同时移动
        {
            fast = fast -> next;
            slow = slow -> next;
        }
        slow -> next = slow -> next -> next;
        return dummyHead -> next;
    }
};
```

### 链表相交 leetcode 面试题 02.07

题目：给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。

图示两个链表在节点 c1 开始相交：



题目数据 保证 整个链式结构中不存在环。

注意，函数返回结果后，链表必须 保持其原始结构 。

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

首先计算两条链表的长度，分别为A，B。 先将长的链表遍历A-B个节点，然后逐一对比两个指针是否相同，如果相同则找到相交点，否则继续向后遍历。

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* curA = headA;
        ListNode* curB = headB;
        int lenA = 0;
        int lenB = 0;
        while(curA != NULL)
        {
            lenA++;
            curA = curA -> next;
        }

        while(curB != NULL)
        {
            lenB++;
            curB = curB -> next;
        }

        curA = headA;
        curB = headB;

        if (lenB > lenA) {	//比较链表长度，设置curA永远为最长的链表。
            swap (lenA, lenB);
            swap (curA, curB);
        }
        // 求长度差
        int gap = lenA - lenB;
        // 让curA和curB在同一起点上（末尾位置对齐）
        while (gap--) {
            curA = curA->next;
        }

        while(curA != NULL && curB != NULL)
        {
            if(curA == curB)
            {
                return curA;
            }
            else
            {
                curA = curA -> next;
                curB = curB -> next;
            }
        }
        return NULL;
    }
};
```

### 环形链表II leetcode 142

题目：给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

不允许修改 链表。

示例 1：

输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/linked-list-cycle-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

思路：

首先需要判断链表是否存在环！通过设置快慢指针，slow每次走一步，fast每次走两步，如果他们相遇则代表存在环。

确定存在环之后，判断其环入口。设置head到环入口长度为 x，环入口到fast与slow相遇到节点为 y，相遇点到环入口长度为 z。 可得：
$$
(x + y) * 2 = x + y + n (y + z)
$$
其中，n为fast在环中转的圈数。化简之后可得：
$$
x = (n-1)(y+z)+ z
$$
所以，链表中 环入口节点🟰 环中fast与slow相遇节点出发的指针与从head出发的指针相遇的节点。



```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while(fast != NULL && fast -> next != NULL)	//排除无环
        {
            fast = fast -> next -> next; //快慢指针找出相遇点
            slow = slow -> next;

            if(slow == fast) //相遇后分别从head与相遇点出发，index相遇点为环入口
            {
                ListNode* index1 = head;
                ListNode* index2 = slow;
                while(index1 != index2)
                {
                    index1 = index1 -> next;
                    index2 = index2 -> next;
                }
                return index1;
            }
        }
        return NULL;
    }
};
```

