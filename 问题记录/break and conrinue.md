## 1. break：

**直接跳出**当前循环体（while、for、do while）或程序块（switch）。其中switch case执行时，一定会先进行匹配，匹配成功返回当前case的值，再根据是否有break，判断是否继续输出，或是跳出判断（可参考[switch的介绍](http://www.cnblogs.com/huiAlex/p/6240844.html)）。

## 2. continue：

不再执行循环体中continue语句之后的代码，直接进行下一次循环。