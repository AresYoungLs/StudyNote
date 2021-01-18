# SqlServer 数据库常用命令

### 删除原表数据,并重置自增列

- truncate table tablename --truncate 方式也可以重置自增字段

---

### 重置表的自增字段,保留数据

- DBCC CHECKIDENT (tablename,reseed,0)

---

### 设置允许显式插入自增列

- SET IDENTITY_INSERT tablename ON

---

### 当然插入完毕记得要设置不允许显式插入自增列

- SET IDENTITY_INSERT tablename Off

---
