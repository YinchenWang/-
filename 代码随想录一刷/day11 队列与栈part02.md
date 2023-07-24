#  Day11 队列与栈 part02

### 有效的括号 leetcode 20

题目：给定一个只包括 `'('`，`')'`，`'{'`，`'}'`，`'['`，`']'` 的字符串 `s` ，判断字符串是否有效。

有效字符串需满足：

1. 左括号必须用相同类型的右括号闭合。
2. 左括号必须以正确的顺序闭合。
3. 每个右括号都有一个对应的相同类型的左括号。 

**示例 1：**

```
输入：s = "()"
输出：true
```

思路：使用栈来处理。考虑总共有几种不匹配情况。

1、长度为奇数，必然不匹配，false。
2、遍历完所有的字符串之后，发现栈不为空，则存在不匹配，false。
3、遍历字符串过程中发现不匹配字符，false。
4、在遍历的过程中，栈已经为空而字符串没有遍历完，说明存在右括号不匹配的情况，false。

```c++
class MyQueue {
public:
    std::stack<int> stackIn;
    std::stack<int> stackOut;

    MyQueue() {

    }
    
    void push(int x) {
        stackIn.push(x);
    }
    
    int pop() {
        if(stackOut.empty())
        {
            while(!stackIn.empty())
            {
                stackOut.push(stackIn.top());
                stackIn.pop();
            }
        }
        int res = stackOut.top();
        stackOut.pop();
        return res;
    }
    
    int peek() {
        int res = this->pop(); // 直接使用已有的pop函数
        stackOut.push(res); // 因为pop函数弹出了元素res，所以再添加回去
        return res;
    }
    
    bool empty() {
        return stackIn.empty() && stackOut.empty();
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```

### 删除字符串中的所有相邻重复项 leetcode 1047

题目：给出由小写字母组成的字符串 `S`，**重复项删除操作**会选择两个相邻且相同的字母，并删除它们。

在 S 上反复执行重复项删除操作，直到无法继续删除。

在完成所有重复项删除操作后返回最终的字符串。答案保证唯一。

 

**示例：**

```
输入："abbaca"
输出："ca"
解释：
例如，在 "abbaca" 中，我们可以删除 "bb" 由于两字母相邻且相同，这是此时唯一可以执行删除操作的重复项。之后我们得到字符串 "aaca"，其中又只有 "aa" 可以执行重复项删除操作，所以最后的字符串为 "ca"。
```



思路：很简单的栈的应用，只要遍历字符串，在该 字符！=栈顶元素时压栈，相等是pop即可。 注意最后的结果字符串要进行反转！！！

```C++
class Solution {
public:
    string removeDuplicates(string s) {
        std::stack<char> st;
        for(char S : s)
        {
            if(st.empty() || S != st.top())
            {
                st.push(S);
            }
            else
            {
                st.pop();
            }
        }
        string res = "";
        while( !st.empty())
        {
            res += st.top();
            st.pop();
        }
        reverse (res.begin(), res.end()); // 此时字符串需要反转一下
        return res;
    }
};
```

### 逆波兰表达式 leetcode 150

逆波兰表达式是一种后缀表达式，所谓后缀就是指算符写在后面。

- 平常使用的算式则是一种中缀表达式，如 `( 1 + 2 ) * ( 3 + 4 )` 。
- 该算式的逆波兰表达式写法为 `( ( 1 2 + ) ( 3 4 + ) * )` 

逆波兰表达式主要有以下两个优点：

- 去掉括号后表达式无歧义，上式即便写成 `1 2 + 3 4 + * `也可以依据次序计算出正确结果。
- 适合用栈操作运算：遇到数字则入栈；遇到算符则取出栈顶两个数字进行计算，并将结果压入栈中

题目：给你一个字符串数组 `tokens` ，表示一个根据 [逆波兰表示法](https://baike.baidu.com/item/逆波兰式/128437) 表示的算术表达式。

请你计算该表达式。返回一个表示表达式值的整数。

**注意：**

- 有效的算符为 `'+'`、`'-'`、`'*'` 和 `'/'` 。
- 每个操作数（运算对象）都可以是一个整数或者另一个表达式。
- 两个整数之间的除法总是 **向零截断** 。
- 表达式中不含除零运算。
- 输入是一个根据逆波兰表示法表示的算术表达式。
- 答案及所有中间计算结果可以用 **32 位** 整数表示。

思路：遍历字符串，遇见数字则压栈，遇见运算符计算然后压栈。

```C++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        std::stack<long long> st;
        for( int i = 0; i < tokens.size(); i ++)
        {
            if(tokens[i] == "+" || tokens[i] == "-" || tokens[i] == "*" || tokens[i] == "/")
            {
                long long num1 = st.top();
                st.pop();
                long long num2 = st.top();
                st.pop();
                if (tokens[i] == "+") st.push(num2 + num1);
                if (tokens[i] == "-") st.push(num2 - num1);
                if (tokens[i] == "*") st.push(num2 * num1);
                if (tokens[i] == "/") st.push(num2 / num1);
            }
            else
            {
                st.push(stoll(tokens[i]));  //stoll 将字符串转换为long long类型
            }
        }
        int result = st.top();
        st.pop();
        return result;
    }
};
```

