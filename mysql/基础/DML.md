# DML 
## 数据操作语言
  >插入  
  删除  
  修改  

# INSERT 插入

## 语法
```sql
INSERT into 表名(列名1, 列名2, ......) 
values(值1, 值2, ......);

INSERT into 表名
SET 列名1 = 值1, 列名2 = 值2, ...... 
```
### 示例1
  `INSERT INTO stu(name, birth, class_id) values('啦啦啦', '2019-10-30', 1005);`

### 示例2
`INSERT INTO stu set name = 'xk', birth = '2555-05-05' class_id = 1005`;

### 两种比较
```sql
--方式1能插入多行, 方式2不支持
INSERT INTO stu 
values('啦啦啦', '2019-10-30', 1005)
values('啦啦啦', '2019-10-30', 1005)
values('啦啦啦', '2019-10-30', 1005);


--方式1支持子查询， 方式2不支持
INSERT INTO stu
SELECT * FROM stu;

#简单表复制
INSERT INTO b_user ( `name`, `password`, `status` )
SELECT `name`, `password`, `status` 
FROM b_user
```
# UPDATE 更新
## 语法
```sql
--语法1
UPDATE 表名
SET 列 = 新值, 列 = 新值, ......
WHERE 筛选条件;

UPDATE stu
SET class_id = 1002
WHERE stu_id = 10;

--语法2, 直接看例子吧
# 修改 班级名称为 网工2班 的班级所有学生的age为18
UPDATE stu s
INNER JOIN class c ON s.class_id = c.class_id
SET s.age = 18
WHERE c.class_name = '网工2班';
```
# DELETE删除

## 语法
```sql
DELETE FROM 表名 WHERE 筛选条件;

TRUNCATE table 表名;
```

### 示例1
```sql
--删除姓王的学生
DELETE FROM stu WHERE stu.name  LIKE '王%';
```

### 示例2
```sql
--删除班级名称为 大数据1班  的学生
DELETE s
FROM stu s
INNER JOIN class c ON s.class_id = c.class_id
WHERE c.class_name = '大数据1班';
```

### 示例3
```sql
#删除班级名称为 计算机科学与技术1班 班级以及学生信息
DELETE class, stu
FROM class
INNER JOIN stu ON stu.class_id = class.class_id
WHERE class.class_name = '计算机科学与技术1班';
```

### 差异
>delete 删除可以加where条件，truncate不能加

>truncate 效率高一点

>truncate可以将自增长列复原，delete则不能

>truncate 删除后不能回滚，delete删除可以回滚

>truncate删除没有返回值，delete删除有返回值