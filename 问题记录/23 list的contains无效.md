实际上执行的是indexOf方法，当list为空时永远为false

```java
public boolean contains(Object o) {
    return indexOf(o) >= 0;
}
```

```java
public int indexOf(Object o) {
    if (o == null) {
        for (int i = 0; i < size; i++)
            if (elementData[i]==null)
                return i;
    } else {
        for (int i = 0; i < size; i++)
            if (o.equals(elementData[i]))
                return i;
    }
    return -1;
}
```