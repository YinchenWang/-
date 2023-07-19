#  day06 哈希表part01

### 有效的字母异位词 leetcode 242

题目：给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。 

示例 1:

输入: s = "anagram", t = "nagaram"
输出: true

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/valid-anagram
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。。

#### 哈希表法

数组就是一个最简单的哈希表，创建一个包含26字母的数组letters，将字符串s映射到上面，对应位置+1，再将字符串t映射到上面，对应位置-1，最后查看letters的值是否全部为0。

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int letters[26] = { 0 };
        int i, sum = 0;
        for(i=0; i<s.size(); i++)
        {
            letters[s[i] - 'a']++;
        }
        for(i=0; i<t.size(); i++)
        {
            letters[t[i] - 'a']--;
        }
        for(i=0; i<26; i++)
        {
            if(letters[i] != 0)
                return false;
        }
        return true;
    }
};
```

### 两个数组的交集 leetcode 349

题目：给定两个数组 nums1 和 nums2 ，返回 它们的交集 。输出结果中的每个元素一定是 唯一 的。我们可以 不考虑输出结果的顺序 。

示例 1：

输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
示例 2：

输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
解释：[4,9] 也是可通过的

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/intersection-of-two-arrays
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

考虑使用哈希表来解决。所以类似查找某个元素是否存在某个容器中的题目都可使用哈希表快速求解。

C++提供了数组，set，map三种哈希表数据结构。这里使用set中的unordered_set最好。其底层通过hash table实现，同时可以自动去重。

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set; // 存放结果，之所以用set是为了给结果集去重
        unordered_set<int> nums_set(nums1.begin(), nums1.end());
        for (int num : nums2) {
            // 发现nums2的元素 在nums_set里又出现过
            if (nums_set.find(num) != nums_set.end()) {
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(), result_set.end());
    }
};
```

### 快乐数 leetcode 202

题目：编写一个算法来判断一个数 n 是不是快乐数。

「快乐数」 定义为：

对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和。
然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。
如果这个过程 结果为 1，那么这个数就是快乐数。
如果 n 是 快乐数 就返回 true ；不是，则返回 false 。

示例 1：

输入：n = 19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/happy-number
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

使用哈希法，用unordered_set存储每个sum，一旦出现重复的sum则该数不是快乐数。直至sum满足快乐数。

```c++
class Solution {
public:
    int getSum(int n)
    {
        int sum = 0;
        while(n)
        {
            sum += (n % 10) * (n % 10);
            n /=  10;
        }
        return sum;
    }

    bool isHappy(int n) {
        unordered_set<int> set;
        while(1)
        {
            int sum = getSum(n);
            if(sum == 1)
            {
                return true;
            }
            if (set.find(sum) != set.end()) {
                return false;
            } else {
                set.insert(sum);
            }
            n = sum;
        }
    }
};
```

### 两数之和 leetcode 1

题目：给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

 

示例 1：

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/two-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

思路：

同样适用哈希法，由于需要得知下标与数值，所有使用map来存储结果。遍历元素，并在map中寻找是否有满足条件的值，如果没找到就将这个值与下标存入map，继续向后遍历。当遍历后方元素时，在map中找到对应的值则返回其对应的下标。



```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        std::unordered_map <int,int> map;
        for(int i = 0; i < nums.size(); i++) {
            // 遍历当前元素，并在map中寻找是否有匹配的key
            auto iter = map.find(target - nums[i]); 
            if(iter != map.end()) {
                return {iter->second, i};
            }
            // 如果没找到匹配对，就把访问过的元素和下标加入到map中
            map.insert(pair<int, int>(nums[i], i)); 
        }
        return {};
    }
};
```

