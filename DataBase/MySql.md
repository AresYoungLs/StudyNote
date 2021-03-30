# MySQL

## MySql 数据库常用命令

### 重置 Mysql 自增列的开始序号

- ALTER TABLE TableName AUTO_INCREMENT = 5
  > 代表重新从 5 开始（包括 5）>

### 清空表数据并且重置自增序列

truncate table [表名]
