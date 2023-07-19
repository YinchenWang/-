#  day02 数组part02

### 有序数组的平方 leetcode 977

给你一个按 **非递减顺序** 排序的整数数组 `nums`，返回 **每个数字的平方** 组成的新数组，要求也按 **非递减顺序** 排序。

**可以暴力求解，平方后再排序。消耗大。使用双指针法，从两头往中间平方，将平方后的数放入新数组的末尾，即可平方的同时完成排序**

#### 前后双指针法

```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int left = 0;
        int right = nums.size() - 1;
        int k = right;
        vector<int> res(nums.size(), 0);
        while (left <= right)
        {
            if(nums[left] * nums[left] <= nums[right] * nums[right])
            {
                res[k--] = nums[right] * nums[right];
                right--;
            } 
            else
            {
                res[k--] = nums[left] * nums[left];
                left++;
            }
        }
        return res;
    }
};
```

### 长度最小的子数组 leetcode 209

题目：给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/minimum-size-subarray-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

使用暴力法需要两个for循环，复杂度O（n^2）

#### 滑动窗口法

通过不断的调整子序列起始位置和终止位置来变换序列长度，同时查找满足条件的序列。

要注意for中使用的是while， 如果target=100而数组为[1，1，1，1，100]，使用if则无法满足条件。

```c++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int res = INT32_MAX;
        int i = 0; //滑动窗口起始位置
        int sum = 0; //滑动窗口和
        int subLength = 0; // 滑动窗口长度；
        for( int j=0; j<nums.size(); j++)
        {
            sum += nums[j]; //计算窗口内的数之和，
            while( sum >= target) //当窗口内数的和 >  target时，处理窗口的起始位置与最小长度
            {
                subLength = j - i + 1; //获取长度， 窗口终止位置 - 起始位置 + 1。
                res = res < subLength ? res : subLength; //获取最小值
                sum -= nums[i++]; //向右移动i，寻找更短的子序列。 注意i++的作用。
            }
        }
        // 如果result没有被赋值的话，就返回0，说明没有符合条件的子序列
        return res == INT32_MAX ? 0 : res;
    }
};
```

### 螺旋矩阵II leetcode 59

题目：给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。

 思路：本题需要注意的是模拟循环时区间边界的处理情况。只用全部左闭右开的区间即可。

**示例 1：** n = 3

![img](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

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

