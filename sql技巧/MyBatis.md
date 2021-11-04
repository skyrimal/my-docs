错误的写法`'#{userNamePinyin}%'`，正确的写法`#{userNamePinyin}'%'`(错误的写法)。





java sql语句 like%?%报错的问题

在数据库中不会报错，但用java调用时确保错。

SQL语句：

```sql
1 SELECT pageId,`name`,text FROM Page WHERE `name` LIKE CONCAT('%',?,'%')
```



varchar转date

```sql
to_date(#{startTime},'YYYY-MM-DD HH24:MI:SS')
```

