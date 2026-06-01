# SQL 学习
## 核心概念
数据库(Database)、表(Table)、行(Row)、列(Column)、主键(Primary Key) -- 行号(唯一标识)、外键(Foregin Key) -- 指向另外一张表的引用

## 基础语句学习
1. 核心概念
- 表：存储数据的格子，类似 Excel 工作表（行=记录， 列=字段）
- 主键：唯一标识一条记录的列（如 user_id）, 通常自动递增
- SQL: 不区分大小写，但习惯关键字大写，表名/字段名小写(具体看数据库)

2. 查询数据(SELECT) -- 最常用
```sql
-- 查询所有列
SELECT * FROM 表名；
-- 查询指定列
SELECT 列1, 列2 FROM  表名;

-- 带条件查询
SELECT * FROM 表名 WHERE 条件;
-- 例子：查询年龄大于18的用户
SELECT * FROM users WHRER age > 18;

-- 多条件：AND / OR 
SELECT * FROM users WHERE age > 18 AND city = "北京";

-- 模糊匹配(LIKE)
SELECT * FROM users WHERE name LIKE "张%"； -- 张开头
SELECT * FROM users WHERE email LIKE "%@qq.com" -- 以 @qq.com 结尾

-- 排序(ORDER BY)
-- 降序
SELECT * FROM users ORDER BY age DESC;
-- 升序（默认）
SELECT * FROM users ORDER BY age ASC;

-- 限制返回行数(分页常用)

-- 取 10 条
SELECT * FROM users LIMIT 10;
-- 跳过20条，取10条(第三页)
SELECT * FROM users LIMIT 10 OFFSET 20;

-- 计算总数(COUNT)
SELECT COUNT(*) FROM users WHERE age > 18;

```

3. 插入数据(INSERT)
```sql
-- 插入一条完整记录（所有列按顺序）
INSERT INTO 表明 VALUES (值1，值2，值3);

-- 插入一条数据 指定要插入的数据列，排除 自增id列的插入防止报错
INSERT INTO STUDENT_INFO （STUDENT_NO, STUDENT_NAME,GENDER，BIRTHDAY，MAJOR，CREATE_TIME，UPDATE_TIME) VALUES(1，'小明'，'M'，'200-10-01'，'电子信息工程技术'，NOW()，NOW());
```
注意：主键列(如id)通常自动生成，不要手动写值。

4. 更新数据(UPDATE)
```sql
-- 修改满足条件的记录
UPDATE 表明 SET 列1 = 新值 WHERE 条件;
-- 例子：把张三的年龄改为26
UPDATE users SET age = 26 WHERE = '张三';

-- 更新多列
UPDATE user SET age = 27, city = '北京' WHERE name = '张三';
-- 更新多列
```

5. 删除数据(DELETE)
```sql
-- 删除满足条件的记录
DELETE FROM 表明 WHERE 条件;
-- 例子：删除张三的记录
DELETE FROM users WHERE name = '张三';
```

## 创建表
1. 创建学生表
```sql
--创建学生表 （表名和字段名都用大写，避免引号问题）
CREATE TABLE "STUDENT_INFO"(
"STUDENT_ID"  INT IDENTITY(1, 1) PRIMARY KEY, -- 自增主键，自增整数主键，起始值1，步长1
"STUDENT_NO" VARCHAR(32) NOT NULL UNIQUE,     -- 学号 唯一约束 非空且唯一
"STUDENT_NAME" VARCHAR(64) NOT NULL,          -- 姓名  非空
"GENDER" CHAR(1),                             -- 性别：F/M 固定长度1字符
"BIRTHDAY" DATE,                              -- 生日 
"MAJOR" VARCHAR(128),                         -- 专业
"CREATE_TIME" TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- 创建时间 默认当前时间戳
"UPDATE_TIME" TIMESTAMP DEFAULT CURRENT_TIMESTAMP. -- 更新时间 默认当前时间戳
```

## 注释
单行注释： -- （连哥哥连字符+空格） 最常用
单行注释： #
|   注释类型   |    语法    |   适用数据库    |  说明    |
|---------------|------------|------------|------------|
| 单行注释 | -- （两个连字符 + 空格）  | 所有主流数据库 | 最常用，从 -- 到行尾都是注释  |
| 单行注释 | # | MySQL | MySQL 特有写法，不推荐跨数据库适用  |
| 多行注释 | /* */ | 所有主流数据库 | 可跨多行，也可嵌入行内  |
