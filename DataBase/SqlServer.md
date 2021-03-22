# SqlServer 数据库常用命令

### 删除原表数据,并重置自增列

- truncate table **tableName** 

---

### 重置表的自增字段,保留数据 （重置自增序列从某个值开始）

- DBCC CHECKIDENT (**tableName**,reseed,0)

---

### 设置允许显式插入自增列

- SET IDENTITY_INSERT **tableName** ON

---

### 当然插入完毕记得要设置不允许显式插入自增列

- SET IDENTITY_INSERT **tableName** Off

---

### 从一个表中查询数据插入另一个表中

<font color="#error">只适用于创建新表的插入（Mysql不支持）</font> <br>
<font color="#error">此语句只能创建现有的准备插入的数据列的表结构</font> <br>
<font color="#error">并且在查询的时候，column_name(s)外面不能够加括号</font> <br>
SELECT **(*)/column_name(s)**INTO**tableName**FROM**sourcetableName**  

<font color="#error">查询添加到已有表</font> <br>
INSERT INTO **tableName** [空]/(字段1，2，3)  SELECT  */字段1，2，3 FROM  **sourcetableName** <font color="red">这里查询的时候需要把字段的数据顺序对应上去</font>