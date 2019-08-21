# DDL
## 介绍
  >数据定义语言  
  库和表的管理

### 库的管理
  >创建  
  修改  
  删除
### 表的管理
  >创建  
  修改  
  删除

### 创建
  >CREATE
### 修改
  >ALTER
### 删除
  >DROP

# 库的管理
## 语法
```sql
CREATE DATABASE book;
#上面的有可能会乱码
#最好用
CREATE DATABASE book charset = utf8;

CREATE DATABASE IF NOT EXISTS book charset = utf8;
```

### 库的修改，一般不用
```sql
ALTER DATABASE book CHARACTER SET gbk;
```

### 删除库
```sql
DROP DATABASE IF EXISTS book;
```

# 表管理

## 语法
```sql
#创建表
CREATE TABLE IF NOT EXISTS 表名(
    列名 列的类型 [(长度) 约束],
    列名 列的类型 [(长度) 约束],
    列名 列的类型 [(长度) 约束],
    ......
    列名 列的类型 [(长度) 约束],
);

CREATE TABLE book(
    id INT, #编号
    bookName VARCHAR(20),#图书名称
    price DOUBLE,#价格
    authorId INT, #作者编号
    publishDate DATETIME#出版日期
);
```

## 表的修改
>### 1. 修改列名
`ALTER TABLE book CHANGE [COLUMN] publishdate publishDate DATETIME;`   column 可省略
>### 2. 修改列的类型或约束
`ALTER TABLE boook MODIFY COLUMN publishdata TIMESTAMP;`将时间类型修改成时间戳格式
>### 3. 添加新列 
`ALTER TABLE book ADD COLUMN color VARCHAR(20);`
>### 4. 删除列
`ALTER TABLE book DROP COLUMN color;`
>### 5. 修改表名
`ALTER TABLE book RENAME TO books;`

## 表的删除
### 语法
```sql
DROP TABLE IF EXISTS books;
```
## 表的复制
### 语法
```sql
#1.仅复制表的结构
CREATE TABLE book_copy LIKE book;

#2.复制结构和数据
CREATE TABLE book_copy2 SELECT * FROM book;

--复制一部分
CREATE TABLE book_copy3 SELECT id, name FROM book;

CREATE TABLE book_copy4 SELECT * FROM book WHERE id = 1;

CREATE TABLE book_copy5 SELECT id, name FROM book WHERE 0;
```

## 常见数据类型
>### 数值型：
    整型:
        类型            字节
        tinyint          1
        smallint         2
        mediumint        3
        int, integer     4
        bigint           8
    小数：
        定点数
        浮点数

#### 如何设置无符号和有符号
```SQL
--创建表的时候默认为有符号
CREATE TABLE tab(
    t1 INT,
    t2 INT UNSIGNED
);
--0填充
CREATE TABLE tab2(
    t1 INT(7) ZEROFILL
);
--这里的长度代表显示的最大宽度，如果不够，用0填充
#不是很懂
```
#### 如果插入的值超过了范围，则会报错，并插入的数值为临界值
#### 如果不设置长度，则会有默认长度
>### 字符型：
    较短的文本： char varchar
    较长的文本：text blod(较长的二进制数据)
>### 日期型
    datetime
数据类型就不继续看了


# 常见约束
## 六大约束
```sql
NOT NULL 非空，用于保证该字段的值不为空 
DEFAULT 默认，用于保证该字段有默认值
PRIMARY KEY  主键，用于保证字段具有唯一性，并且非空
UNIQUE  唯一，用于保证该字段具有唯一性，可以为空
CHECK 检查约束，【mysql中不支持，兼容其它数据库】
FOREIGN KEY 用于限制两个表的关系，用于保证该字段的值必须来自主表关联列的值


添加约束的时机：
  1.创建表的时候
  2.修改表的时候

约束的分类
  1.表级约束
          除了非空，默认其它都支持
  2.列级约束
          六大约束语法都支持列级约束，但外键约束没有效果
```

## 列级约束
```sql
CREATE TABLE class(
  id PRIMARY KEY,
  class_name UNIQUE
);

CREATE TABLE stu(
    id  INT PRIMARAY KEY,
    stu_name VARCHAR(20) NOT NULL,
    gender CHAR(1) CHECK(gender='男' OR gender='女'),#检查约束，这里写上，虽然没有用
    stu_id VARCHAR(12) UNIQUE,--唯一约束
    age INT DEFAULT 18, --默认约束
    class_id INT FOREIGN KEY REFERENCES class(id)
);
```

## 表级约束
```sql
CREATE TABLE IF NOT EXISTS stu(
  id  INT,
  stuName VARCHAR(20),
  gender CHAR(1),
  stu_id  VARCHAR(12),
  age INT,
  class_id  INT,

  CONSTRAINT pk PRIMARY KEY(id),#主键
  CONSTRAINT uq UNIQUE(stu_id),
  CONSTRAINT ck CHECK(gender='男' OR gender='女'),
  CONSTRAINT fk_stu_class FOREIGN KEY(class_id) REFERENCES class(id)
);


--外键必须是被引用表的主键或者unique，删除的时候要按照顺序删除

```

## 修改表的时候添加约束
```sql
CREATE TABLE stu(
  id  INT,
  stuName VARCHAR(20),
  stu_id  VARCHAR(12),
  age INT,
  class_id  INT
);
--添加非空约束
ALTER TABLE stu  MODIFY COLUMN stuName VARCHAR(20) NOT NULL;
--添加默认约束
ALTER TABLE stu MODIFY COLUMN age INT DEFAULT 18;
--添加主键
  --列级约束
  ALTER TABLE stu MODIFY COLUMN id INT PRIMARY KEY;
  --表级约束
  ALTER TABLE stu ADD PRIMARY KEY(id);

--添加唯一
  -- 列级约束
  ALTER TABLE stu MODIFY COLUMN stu_id VAARCHAR(20) UNIQUE;
  --表级约束
  ALTER TABLE stu ADD UNIQUE(stu_id);

  --添加外键， 列级的外键没效果
  ALTER TABLE stu ADD CONSTRAINT fk_class_id_class FOREIGN KEY(class_id) REFERENCE class(id);
```
## 修改表的时候删除约束
```sql
--删除非空约束
ALTER TABLE stu MODIFY COLUMN stuName VARCHAR(20) NULL;
--删除默认约束
ALTER TABLE stu MODIFY COLUMN age INT;
--删除主键
ALTER TABLE stu DROP PRIMARY KEY;
--删除唯一
ALTER TABLE stu DROP INDEX stu_id;
--删除外键约束
ALTER TABLE stu DROP FOREIGN KEY fk_class_id_class;
```


# 标识列
    标识列又称为自称长列
    含义：可以不用手动插入值，系统提供默认的序列值
## 创建表时候设置标识列
```SQL
CREATE TABLE test(
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20)
);

SHOW VARIABLES LIKE '%auto_increment%'; 
--可以设置步长，不能设置起始值
SET auto_increment_increment=3;



--特点：
-- 标识列不一定要和主键搭配，但要求必须是一个key
-- 一个表中最多只能有一个标识列
-- 标识列类型只能是数值型
-- 标识列可以通过SET auto_increment_increment=3;设置步长
```

## 修改表的时候设置标识列
```sql
ALTER TABLE stu MODIFY COLUMN id INT PRIMARY KEY AUTO_INCREMENT;
```