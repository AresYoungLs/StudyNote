# MySQL

## MySql 数据库常用命令

### 重置 Mysql 自增列的开始序号

- ALTER TABLE TableName AUTO_INCREMENT = 5
  > 代表重新从 5 开始（包括 5）>

### 清空表数据并且重置自增序列

truncate table [表名]

### 找回删除数据
```sql
--开启行移动功能 
 ·alter table 表名 enable row movement
 --恢复表数据
 ·flashback table 表名 to timestamp to_timestamp(删除时间点','yyyy-mm-dd hh24:mi:ss')
 --关闭行移动功能 ( 千万别忘记 )
 ·alter table 表名 disable row movement
```