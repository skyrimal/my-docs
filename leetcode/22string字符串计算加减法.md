# [ 基本计算器](https://leetcode-cn.com/problems/basic-calculator/)

实现一个基本的计算器来计算一个简单的字符串表达式 `s` 的值。

    提示：
    1 <= s.length <= 3 * 105
    s 由数字、'+'、'-'、'('、')'、和 ' ' 组成
    s 表示一个有效的表达式

1.入容器栈

2.遇到‘)’出容器栈入计算栈

3.计算栈计算后再入容器栈

```java
public int calculate(String s) {
    //由提示只，需要先去除所有的' '
    String repS = s.replaceAll(" ", "");
    //可以用两个栈来进行计算，一个保存一个反向取出进行计算
    char[] chars = repS.toCharArray();
    return numberMoveAndMath(chars);
}

private int numberMoveAndMath(char[] chars) {
    int length = chars.length;
    Stack<Character> contentStack = new Stack<>();
    Stack<Character> mathStack = new Stack<>();
    for (char aChar : chars) {
        if (aChar != ')') {
            contentStack.push(aChar);
            continue;
        }
        while (contentStack.peek() != '(') {
            mathStack.push(contentStack.pop());
        }
        int tempNum = math(mathStack);
        contentStack.pop();//丢弃左括号
        insertNumChar(tempNum, contentStack);//计算结果再插入
        if (contentStack.size() == 0) return tempNum;
    }
    while (contentStack.size() != 0) {
        mathStack.push(contentStack.pop());
    }
    return math(mathStack);
}

private void insertNumChar(int num,Stack<Character> contentStack){
    char[] chars = (num + "").toCharArray();
    for (char aChar : chars) {
        contentStack.push(aChar);
    }
}

private int math(Stack<Character> mathStack) {
    //初始化第一个数
    int num1 = getNum(mathStack);
    while (mathStack.size()!=0){
        Character symbol = mathStack.pop();
        int num2 = getNum(mathStack);
        num1=mathAction(num1,num2,symbol);
    }
    return num1;
}

private int getNum(Stack<Character> mathStack) {
    int symbol=1;
    if(mathStack.peek()=='-'){
        mathStack.pop();
        symbol=-1;
    }
    int num1 = 0;
    while (mathStack.size()>0&&mathStack.peek() != '+' && mathStack.peek() != '-') {
        num1 = num1 * 10 + Character.getNumericValue(mathStack.pop());
    }
    return num1*symbol;
}

private int mathAction(int num1, int num2, Character symbol) {
    switch (symbol) {
        case '+':
            return num1 + num2;
        case '-':
            return num1 - num2;
        default:
            return 0;
    }
}
```