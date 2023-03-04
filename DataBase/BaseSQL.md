# SQL server

## ISNULL NULLIF 和 IFNULL 函数的区别

### ISNULL

ISNULL(expr) 如果expr 为NULL，那么ISNULL() 的返回值为 1，否则返回值为0  -- mysql<br>
ISNULL(expr1，expr2)  如果数据为null 则返回expr2，否则返回本身 --SQLserver

### NULLIF(expr1,expr2)

若expr1等于expr2，则返回null，否则返回expr1

### IFNULL(expr1,expr2)

若expr1不为null，则ifnull()的返回值为expr1；若expr1为null，则返回expr2的值<br>

```diff
- sql server 无此函数
```
