---
title: Day03 链表 part01
date: 2023-7-15
tags:
 - algorithms
categories:
 - Algorithms
---
#  Day03 链表part01

### 移除链表元素 leetcode 203

题目：给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。


示例 1：


输入：head = [1,2,6,3,4,5,6], val = 6
输出：[1,2,3,4,5]

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/remove-linked-list-elements
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

#### 虚拟头节点法

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
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyHead = new ListNode(0);      //使用虚拟头节点，避免头节点是否为空的情况。
        dummyHead -> next = head;
        ListNode* cur = dummyHead;
        while (cur -> next != NULL)
        {
            if (cur -> next -> val == val)      //若下一个节点值为空，则删除该节点。
            {
                ListNode* tmp = cur->next;
                cur -> next = cur -> next -> next;
                delete tmp;
            }
            else                            //否则，遍历下一个节点。
            {
                cur = cur -> next;
            }
        }
        head = dummyHead -> next;       //删除虚拟头节点，将头节点更改为head
        delete dummyHead;
        return head;
    }
};
```

### 设计链表 leetcode 707

题目：你可以选择使用单链表或者双链表，设计并实现自己的链表。

单链表中的节点应该具备两个属性：val 和 next 。val 是当前节点的值，next 是指向下一个节点的指针/引用。

如果是双向链表，则还需要属性 prev 以指示链表中的上一个节点。假设链表中的所有节点下标从 0 开始。

实现 MyLinkedList 类：

MyLinkedList() 初始化 MyLinkedList 对象。
int get(int index) 获取链表中下标为 index 的节点的值。如果下标无效，则返回 -1 。
void addAtHead(int val) 将一个值为 val 的节点插入到链表中第一个元素之前。在插入完成后，新节点会成为链表的第一个节点。
void addAtTail(int val) 将一个值为 val 的节点追加到链表中作为链表的最后一个元素。
void addAtIndex(int index, int val) 将一个值为 val 的节点插入到链表中下标为 index 的节点之前。如果 index 等于链表的长度，那么该节点会被追加到链表的末尾。如果 index 比长度更大，该节点将 不会插入 到链表中。
void deleteAtIndex(int index) 如果下标有效，则删除链表中下标为 index 的节点。


示例：

```c++
输入
["MyLinkedList", "addAtHead", "addAtTail", "addAtIndex", "get", "deleteAtIndex", "get"]
[[], [1], [3], [1, 2], [1], [1], [1]]
输出
[null, null, null, null, 2, null, 3]

解释
MyLinkedList myLinkedList = new MyLinkedList();
myLinkedList.addAtHead(1);
myLinkedList.addAtTail(3);
myLinkedList.addAtIndex(1, 2);    // 链表变为 1->2->3
myLinkedList.get(1);              // 返回 2
myLinkedList.deleteAtIndex(1);    // 现在，链表变为 1->3
myLinkedList.get(1);              // 返回 3
```

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/design-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```c++
class MyLinkedList {
public:
    struct LinkedNode{
        int val;
        LinkedNode* next;
        LinkedNode(int val):val(val), next(nullptr){}
    };
    
    MyLinkedList() {
        _dummyHead = new LinkedNode(0);
        _size = 0;
    }
    
    int get(int index) {
        if (index > (_size - 1) || index < 0) {
            return -1;
        }
        LinkedNode* cur = _dummyHead -> next;
        while (index--)
        {
            cur = cur -> next;
        }
        return cur -> val;
    }
    
    void addAtHead(int val) {
        LinkedNode* tmp = new LinkedNode(val);
        tmp -> next = _dummyHead -> next;
        _dummyHead -> next = tmp;
        _size++;
    }
    
    void addAtTail(int val) {
        LinkedNode* tmp = new LinkedNode(val);
        LinkedNode* cur = _dummyHead;
        while(cur -> next != nullptr)
        {
            cur = cur -> next;
        }
        cur -> next = tmp;
        _size++;
    }
    
    void addAtIndex(int index, int val) {
        LinkedNode* tmp = new LinkedNode(val);
        LinkedNode* cur = _dummyHead;
        if(index > _size) return;
        if(index < 0) index = 0;
        while (index--)
        {
            cur = cur -> next;
        }
        tmp -> next = cur -> next;
        cur -> next = tmp;
        _size++;
    }
    
    void deleteAtIndex(int index) {
        if (index >= _size || index < 0) {
            return;
        }
        LinkedNode* cur = _dummyHead;
        while (index--){
            cur = cur -> next;
        }
        LinkedNode* tmp = cur -> next;
        cur -> next = cur ->next -> next;
        delete tmp;		//此处注意释放tmp，并更改其指针为null！！！
        tmp = nullptr;		//delete后的指针并不是null，而是一个随机的值！！！
        _size--;
    }

private:
    int _size;
    LinkedNode* _dummyHead;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```

### 反转链表 leetcode 206

题目：给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。


示例 1：

输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/reverse-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n, vector<int>(n, 0)); 		//定义一个二维数组
        int startX = 0; //定义行
        int startY = 0; //定义列
        int loop = n / 2;   //循环次数
        int mid = n / 2;    //矩阵中心
        int count = 1;      //值
        int offset = 1;     //偏移量，控制循环右区间（左闭右开）
        int i, j = 0;       //当前行列的值

        while(loop--){
            i = startX;
            j = startY;

            for(j = startY; j < n - offset; j++)        //上行从左到右
            {
                res[startX][j] = count++;
            }
            for(i = startX; i < n - offset; i++)        //右列从上到下
            {
                res[i][j] = count++;
            }
            for( ; j > startY; j--)                     //下行从右往左
            {
                res[i][j] = count++;
            }
            for( ; i > startX; i--)                     //左列从下往上
            {
                res[i][j] = count++;
            }
            startX++;           //增加一行
            startY++;           //增加一列
            offset += 1;        //偏移量
        }

        if (n % 2)              //若n为奇数，单独设置中心点的值；
        {
            res[mid][mid] = count;
        }
        return res;
    }
};
```

