#  day01 数组part01



### 二分法查找 leetcode 704

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/binary-search
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**二分法查找时主要应当注意的问题的查找区间的不同，使用不同的区间则处理方法不同。此处应当格外注意！！！**

#### 左闭右闭区间

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;
        while (left <= right)
        {
            int middle = left + ((right - left) / 2);
            if (nums[middle] > target)
            {
                right = middle - 1;
            }
            else if (nums[middle] < target)
            {
                left = middle + 1;
            }
            else
                return middle;
        }
        return -1;
    }
};
```

#### 左闭右开区间

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();
        while (left < right)
        {
            int middle = left + ((right - left) / 2);
            if (nums[middle] > target)
            {
                right = middle;
            }
            else if (nums[middle] < target)
            {
                left = middle + 1;
            }
            else
                return middle;
        }
        return -1;
    }
};
```

### 移除元素 leetcode 27

#### 题目：

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/remove-element
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

#### 暴力法

使用双重循环，每当遇见一个匹配的值便将数组后的所有数往前移动一位，虽然可解，但是内存消耗很大。

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size = nums.size();
        for(int i=0; i<size; i++)
        {
            if(nums[i] == val)
            {
                for(int j=i+1; j<size; j++)
                {
                    nums[j-1] = nums[j];
                }
                i--;
                size -= 1;
            }
        }
        return size;
    }
};
```

#### 双指针法（快慢指针）

fast指针寻找新的不包含目标值的数组，slow指针指向更新新数组的下标。每当fast找到一个新的数之后，讲slow指向该数

```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slowIndex = 0;
        for( int fastIndex=0; fastIndex<nums.size(); fastIndex++)
        {
            if(val != nums[fastIndex]) //找到新数
            {
                nums[slowIndex++] = nums[fastIndex];//slow++，更新新数组下标
            }
        }
        return slowIndex;
    }
};
```

#### 双指针法（相向双指针法）

left从左到右找等于val 的值，right从右往左找不等于val的值，找到一队之后用rught指向的值覆盖left的值，继续寻找下一队，直至left==right。

```c++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int leftIndex = 0;
        int rightIndex = nums.size() - 1;
        while (leftIndex <= rightIndex) {
            // 找左边等于val的元素,当找到时left指向这个数
            while (leftIndex <= rightIndex && nums[leftIndex] != val){
                ++leftIndex;
            }
            // 找右边不等于val的元素，找到时right指向这个数
            while (leftIndex <= rightIndex && nums[rightIndex] == val) {
                -- rightIndex;
            }
            // 将右边不等于val的元素覆盖左边等于val的元素，继续寻找下一对匹配的
            if (leftIndex < rightIndex) {
                nums[leftIndex++] = nums[rightIndex--];
            }
        }
        return leftIndex;   // leftIndex一定指向了最终数组末尾的下一个元素
    }
};
```

