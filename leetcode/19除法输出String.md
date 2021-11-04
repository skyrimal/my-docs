```java
public static String fractionToDecimal(int numerator, int denominator) {
    StringBuilder res = new StringBuilder();
    /**************特情判断*******/
    if(numerator == 0) return String.valueOf(0);
    if(numerator < 0 != denominator < 0) res.append("-");
    /****************************/
    //1.计数整数
    long num = Math.abs((long)numerator), den = Math.abs((long)denominator);
    res.append(num / den);
    if(num % den == 0) return res.toString();//如果只有整数

    //2.计数小数位
    res.append(".");
    Map<Long, Integer> ht = new HashMap<>(); //记录被除数
    while(num % den != 0) {
        num = num % den * 10;
        if(ht.containsKey(num)) {//当被除数重复时可以肯定出现了无限循环小数
            res.insert(ht.get(num), "(").append(")");
            break;
        }
        ht.put(num, res.length());//记录每个被除数和计算后的数字长度
        res.append(num / den);
    }
    return res.toString();
}
```