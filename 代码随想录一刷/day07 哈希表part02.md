#  day07 哈希表part02

### 四数相加II leetcode 452

题目：给你四个整数数组 nums1、nums2、nums3 和 nums4 ，数组长度都是 n ，请你计算有多少个元组 (i, j, k, l) 能满足：

0 <= i, j, k, l < n
nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0


示例 1：

输入：nums1 = [1,2], nums2 = [-2,-1], nums3 = [-1,2], nums4 = [0,2]
输出：2
解释：
两个元组如下：
1. (0, 0, 0, 1) -> nums1[0] + nums2[0] + nums3[0] + nums4[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1[1] + nums2[1] + nums3[0] + nums4[0] = 2 + (-1) + (-1) + 0 = 0

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/4sum-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

#### 哈希表法

本题需要采用分组的方法，同时使用哈希法求解。通过将a,b,c,d四个数组两两相加，可以讲题目更改为类似于两数之和形式。由于题目只要求计算有多少个满足条件的元组，因此无需考虑去重的问题。

使用map存储a,b数组中两两相加的结果。key为和，value为出现的次数。然后遍历c,d数组，查找（0 - （c+d))是否存在于map中。如果存在则count += value。

分组之后该问题的时间复杂的为O(n^2)

```c++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> umap;
        for(int a : nums1)
        {
            for(int b : nums2)
            {
                umap[a + b]++;
            }
        }

        int count = 0;
        for(int c : nums3)
        {
            for(int d : nums4)
            {
                if (umap.find(0 - (c + d)) != umap.end()) {
                    count += umap[0 - (c + d)];
                }
            }
        }
        return count;
    }
};
```

### 赎金信 leetcode 383

题目：给你两个字符串：ransomNote 和 magazine ，判断 ransomNote 能不能由 magazine 里面的字符构成。

如果可以，返回 true ；否则返回 false 。

magazine 中的每个字符只能在 ransomNote 中使用一次。 

示例 1：

输入：ransomNote = "a", magazine = "b"
输出：false

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/ransom-note
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

本题第一反应是类似，有效字母异位词。考虑使用map来完成，思路简单明了。看了答案后得知map维持红黑树的消耗大，还是数组方法更简单

```c++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int letters[26] = {0};

        if(ransomNote.size() > magazine.size()) //magazine长度不够组成ransomNote。
        {
            return false;
        }

        for(char a : magazine)
        {
            letters[a - 'a']++;
        }

        for(char b : ransomNote)
        {
            letters[b - 'a']--;

            if(letters[b - 'a'] < 0)
                return false;
        }
        return true;
    }
};
```

### 三数之和 leetcode 15

题目：给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j、i != k 且 j != k ，同时还满足 nums[i] + nums[j] + nums[k] == 0 。请

你返回所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。 

示例 1：

输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/3sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

#### 哈希法

第一想法，分组+哈希法。结果去重火葬场！！！

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        // 找出a + b + c = 0
        // a = nums[i], b = nums[j], c = -(a + b)
        for (int i = 0; i < nums.size(); i++) {
            // 排序之后如果第一个元素已经大于零，那么不可能凑成三元组
            if (nums[i] > 0) {
                break;
            }
            if (i > 0 && nums[i] == nums[i - 1]) { //三元组元素a去重
                continue;
            }
            unordered_set<int> set;
            for (int j = i + 1; j < nums.size(); j++) {
                if (j > i + 2
                        && nums[j] == nums[j-1]
                        && nums[j-1] == nums[j-2]) { // 三元组元素b去重
                    continue;
                }
                int c = 0 - (nums[i] + nums[j]);
                if (set.find(c) != set.end()) {
                    result.push_back({nums[i], nums[j], c});
                    set.erase(c);// 三元组元素c去重
                } else {
                    set.insert(nums[j]);
                }
            }
        }
        return result;
    }
};

```

#### 双指针法

先对数组进行排序，接着使用双指针法。一个指针 i 在循环时指向当前元素 。一个left指向 i+1， 一个right指向size-1. 

当nums[i] + nums[left] + nums[right] > 0 时 right--， 
当nums[i] + nums[left] + nums[right] < 0 时 left++，
当nums[i] + nums[left] + nums[right] = 0 时 保存结果集，

本题最重要的是去重，首先进行i的去重，当nums[i] == nums[i -1]时，舍弃该元组，因为已经存储，不用nums[i] == nums[i +1]是避免类似（-1， -1， 2）元组出现时将其舍弃。同时，当出现（0，-1，-1，-1，1，1，1）类型的数组时，应当只存储一个，所有此处也应当去重。

```C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for(int i=0; i<nums.size(); i++)
        {
            if(nums[i] > 0) return result;      //排序后，当第一个元素>0，则该数组无法满足条件。

            if(i > 0 && nums[i] == nums[i - 1]) continue;   //去重左边元素相同的情况。

            int left = i + 1;
            int right = nums.size() - 1;
            while(left < right)
            {
                if (nums[i] + nums[left] + nums[right] > 0)
                    right --;
                else if (nums[i] + nums[left] + nums[right] < 0)
                    left ++;
                else
                {
                    result.push_back(vector<int>{nums[i], nums[left], nums[right]});    //压入结果
                    while(left < right && nums[left] == nums[left + 1]) //向右去重left
                        left ++;
                    while(left < right && nums[right] == nums[right - 1])   //向左去重right
                        right --;
                    right --;
                    left ++;
                }
            }
        }
        return result;
    }
};

```



### 四数之和 leetcode 18

题目：给你一个由 n 个整数组成的数组 nums ，和一个目标值 target 。请你找出并返回满足下述全部条件且不重复的四元组 [nums[a], nums[b], nums[c], nums[d]] （若两个四元组元素一一对应，则认为两个四元组重复）：

0 <= a, b, c, d < n
a、b、c 和 d 互不相同
nums[a] + nums[b] + nums[c] + nums[d] == target
你可以按 任意顺序 返回答案 。

示例 1：

输入：nums = [1,0,-1,0,-2,2], target = 0
输出：[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/4sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

思路：

思路与三数之和相同，需要再套一个for循环，将三数之和中确定的i，在此变为确定的两个数k，i。注意进行两次剪枝与去重，其余和三数之和一样。



```C++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());

        for(int k=0; k<nums.size(); k++)
        {
            if(nums[k] > 0 && target > 0 && nums[k] > target)   break; //剪枝

            if(k > 0 && nums[k] == nums[k - 1]) continue;   //k去重

            for(int i = k + 1; i < nums.size() - 1; i ++)
            {
                if (nums[k] + nums[i] > target && nums[k] + nums[i] >= 0) break;    //二级剪枝

                if(i > k + 1 && nums[i] == nums[i - 1]) continue;   //i去重

                int left = i + 1;
                int right = nums.size() - 1;
                while(left < right)
                {
                    // nums[k] + nums[i] + nums[left] + nums[right] > target 会溢出
                    if ((long) nums[k] + nums[i] + nums[left] + nums[right] > target) {
                        right--;
                    // nums[k] + nums[i] + nums[left] + nums[right] < target 会溢出
                    } else if ((long) nums[k] + nums[i] + nums[left] + nums[right]  < target) {
                        left++;
                    } else {
                        result.push_back(vector<int>{nums[k], nums[i], nums[left], nums[right]});
                        // 对nums[left]和nums[right]去重
                        while (right > left && nums[right] == nums[right - 1]) right--;
                        while (right > left && nums[left] == nums[left + 1]) left++;

                        // 找到答案时，双指针同时收缩
                        right--;
                        left++;
                    }
                }
            }
        }
        return result;
    }
};
```

