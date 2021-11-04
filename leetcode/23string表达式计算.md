# [ 基本计算器 II](https://leetcode-cn.com/problems/basic-calculator-ii/)

给你一个字符串表达式 `s` ，请你实现一个基本计算器来计算并返回它的值。

整数除法仅保留整数部分。

>1 <= s.length <= 3 * 105
>
>s 由整数和算符 ('+', '-', '*', '/') 组成，中间由一些空格隔开
>
>s 表示一个 有效表达式
>
>表达式中的所有整数都是非负整数，且在范围 [0, 231 - 1] 内
>
>题目数据保证答案是一个 32-bit 整数

```java
public int calculate(String s) {
    Deque<Integer> stack = new LinkedList<>();
    char preSign = '+';
    int num = 0;
    int n = s.length();
    for (int i = 0; i < n; ++i) {
        /*  确定指定的字符是否为数字。
        如果字符的一般类别类型由字符.getType（ch），是十进制数字。
        某些包含数字的Unicode字符范围： */
        if (Character.isDigit(s.charAt(i))) {
            num = num * 10 + s.charAt(i) - '0';
        }
        if (!Character.isDigit(s.charAt(i)) && s.charAt(i) != ' ' || i == n - 1) {
            switch (preSign) {
                case '+':
                    stack.push(num);
                    break;
                case '-':
                    stack.push(-num);
                    break;
                case '*':
                    stack.push(stack.pop() * num);
                    break;
                default:
                    stack.push(stack.pop() / num);
            }
            preSign = s.charAt(i);
            num = 0;
        }
    }
    int ans = 0;
    while (!stack.isEmpty()) {
        ans += stack.pop();
    }
    return ans;
}
```