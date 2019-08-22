基本的sql就不记录了  

>## 查询常量值
```SQL
1. SELECT 100;
2. SELECT 'join';
```
>## 查询表达式
```SQL
1. SELECT 100 * 98;
2. SELECT 100 % 98;  
```
>## 查询函数， 获取函数的返回值
```SQL
1. SELECT VERSION();
```
>## 别名
```SQL
1. SELECT 100 AS 结果;
2. SELECT 100 结果;
```
>## 去重
```SQL
1. SELECT DISTINCT school_id FROM student
```
>## +号作用
```SQL
    select 100 + 200; 两个操作数都为数值型， 则做加法运算
    select '100' + 100;其中有一个为字符型，试图将字符型数值转换成数值型如果转换成功，则继续做加法运算，转换失败，将字符型转换成0
    
    SELECT 100 + 100;       --->200
    SELECT 100 + "100";     --->200
    SELECT 100 + 'J';       --->100
    SELECT null + 100;      --->null 只要一方为null，结果肯定为null
```
>## CONCAT 字符串拼接
```SQL
    SELECT CONCAT('A', 'B', 'C'); --->ABC  
    如果存在null 结果为null 
```    
>## IFNULL
```SQL
    SELECT IFNULL(age, -100) as 年龄;  --如果年龄为null，将显示-100
```
>## 模糊查询
```SQL
    like
    between and
    in
    is null

    学生姓名中包含字符 海
    SELECT * FROM stu WHERE name like '%海%';

    和通配符搭配 % 任意个字符
    _任意单个字符

    查询第二个字符为  海 第五个字符为  小
    SELECT * FROM stu  WHERE name like '_海__小%';

    查询第二个字符为下划线的
    SELECT * FROM stu WHERE name like '_\_%';
    SELECT * FROM stu WHERE name like '_$_%' ESCAPE '$';
    SELECT * FROM stu WHERE name like '_R_%' ESCAPE 'R';

    查询age大于18小于21的同学
    SELECT * FROM stu WHERE age > 18 AND age < 21;
    SELECT * FROM stu WHERE age BETWEEN 18 AND 21;
    IN
    SELECT * FROM stu WHERE id IN (1, 2, 3, 4, 5, 6);
    IS NULL    = 、 <>  不能判断nul值
    SELECT * FROM stu WHERE address IS NULL;

    安全等于 <=>
    可以判断null值
```
>## LENGTH 字节长度
    SELECT LENGTH(name) AS 姓名长度 FROM stu;

># 常见函数
    SELECT 函数名() [from 表];
>## 字符函数
```SQL
    1.LENGTH   //一个汉字三个字节 utf8
      SELECT LENTTH('张');

    2.CONCAT  字符串拼接
      SELECT CONCAT(last_name, ',', first_name) 姓名 FROM stu;

    3. UPPER、LOWER
      SELECT UPPER('jOhn');
      SELECT LOWER('jOhn');

    4.SUBSTR SUBSTRING  索引从1开始
      SELECT SUBSTR('123网络安全好坑', 4) out_put; --->网络安全好坑
      SELECT SUBSTR('123网络安全好坑', 5) out_put; --->络安全好坑

    5.INSTR  返回字符串起始索引
      SELECT INSTR('小明去上学', '明') output;  ----> 2

    6.TRIM
      SELECT LENGTH(TRIM('  小明  ')) output, TRIM('  小明  ') out_put;    --->6 --->小明
      SELECT TRIM('a' FROM 'aaa里aaa面aaa') as output;---->里aaa面

    7.LPAD指定填充
      SELECT LPAD('小明', 6, "*"); ---> ****小明
      SELECT RPAD('小明', 6, "*"); ---> 小明****
```
>## 数学函数
```SQL
    1. ROUND    四舍五入
      SELECT ROUND(-1.55);  --> -2
      SELECT ROUND(1.55);   -->  2
      SELECT ROUND(1.555, 2); --->1.56

    2. CEIL     向上取整    
    3. FLOOR    向下取整

    4. TRUNCATE 截断,从小数点后面位数开始截断
      SELECT TRUNCATE(1.6999, 2); -->1.69

    5. MOD 取模
        SELECT MOD(10, 3);
        SELECT 10 % 3;
```
>## 日期函数
```SQL
    1.NOW 返回当前的时间和日期
      SELECT NOW();       ---> 2019-07-11 12:09:11
    
    2. CURDATE  返回当前的日期
      SELECT CURDATE();   --->2019-07-11
    
    3.CURTIME  返回当前时间
      SELECT CURTIME();   --->12:12:29

    4. 获取年月日，时分秒
      SELECT YEAR(NOW());  --->2019
      SELECT MONTH(NOW()); --->7
      SELECT DATE(NOW());  --->11

    5. STR_TO_DATE 将日期格式的字符串转换成日期格式
      SELECT STR_TO_DATE('2019-7-7', '%Y-%m-%d');
    
    6. DATE_FORMAT 将日期转换成字符
      SELECT DATE_FORMAT(NOW(), "%Y年%m月%d日");
    
    7. DATEDIFF
      SELECT DATEDIFF('2019-7-20',NOW());-->8   今天是2019-7-12号
      SELECT DATEDIFF('2019-7-20','2018-7-20') --->365
```
># 其它函数
    SELECT VERSION();  
    SELECT DATABASE();
    SELECT USER();

># 流程控制函数

>## IF
```SQL
    SELECT IF(10 > 5, '大', '小');   ---> 大
    SELECT address, IF(address IS NULL,  '地址未填写', '已填写') 备注;
```
>## CASE
```SQL
>### 写法
    case 要判断的表达式
    when 常量1 then 要显示的值或者语句
    when 常量1 then 要显示的值或者语句
    else 要显示的值或者语句
    end
>### 运用 1
    公司评级有5个等级，发奖金的时候按照总工资乘以评级除以10来发
    SELECT id, name, all_salary 总工资, grade
    CASE grade
    WHEN 1 THEN all_salary*0.1
    WHEN 2 THEN all_salary*0.2
    WHEN 3 THEN all_salary*0.3
    WHEN 4 THEN all_salary*0.4
    WHEN 5 THEN all_salary*0.5
    ELSE 0
    END AS 奖金
    FROM employees;
>### 运用 2
    如果年龄大于60 显示 老年
    如果年龄大于40 显示 中年
    如果年龄大于20 显示 青年
    如果年龄大于0  显示 少年
    SELECT age
    CASE
    WHEN age > 60 THEN '老年'
    WHEN age > 40 THEN '中年'
    WHEN age > 20 THEN '青年'
    ELSE '少年'
    END AS 年龄段
    FROM employees;
```
>## 分组函数
```SQL
>### 基本5个
    sum   求和
    avg   求平均值
    max   最大值
    min   最小值
    count 计算个数

>### 简单使用
    SELECT AVG(score) 平均分, MAX(score) 最高分, MIN(score) 最低分, COUNT(id) 总人数, SUM(score) 总分 FROM stu;
>### 参数类型支持
    SUM() 如果存在字符型，则将字符型转换成0，其它数值型相加
    AVG() 同上

    MAX() 可以是字符型 第一个字母排序，中文排序的话，emmm不太清楚
    MIN() 可以是字符型
>### 是否忽略null值  ----  null参与运算的都为null
    SUM() null没有参与运算
    AVG() 忽略了null值，并且null也不参与求平均

    其它的也忽略null值
    所有分组函数忽略null

    SELECT SUM(DISTINCT age) 去重, SUM(age) 原数据 from stu;

    SELECT COUNT(DISTINCT age) 有几个不同的年龄 FROM stu；
      --count函数：
      SELECT COUNT(score) FROM stu;  --如果有null，则不会统计
      SELECT COUNT(*) FROM stu;   --统计行数
      SELECT COUNT(1) FROM stu；
      SELECT COUNT('常量')  FROM stu;
      --效率：
      --MYISAM 存储引擎下，COUNT(*) 的效率高
      --INNODB 存储下，COUNT(*) 和 COUNT(1) 效率差不多，比count(字段)效率高
```

      如下查询
```SQL
      SELECT AVG(age), name FROM stu;
      这个查询语句，查询年龄的平均值和name，没有意义
      和分组函数一同查询的字段一般是 group by 后的字段

      需求：得到每个部门的平均工资
      SELECT AVG(salary) 平均工资, department 部门 FROM employees GROUP BY department;

      查询邮箱中包含a字符的，每个部门的平均工资
      SELECT AVG(salary), department_id
      FROM employees
      WHERE email LIKE '%a%'
      GROUP BY department_id;
          WHERE 的位置在 FROM 后面 GROUP BY 的前面

      查询有奖金的每个领导手下员工的最高工资 奖金字段：commission_pct
      SELECT MAX(salary), manager_id
      FROM employees
      WHERE commission_pct IS NOT NULL
      GROUP BY manager_id;

      查询哪个部门的员工个数大于20
      SELECT COUNT(*), department_id 
      FROM employees 
      GROUP BY department_id 
      HAVING COUNT(*)>20;

      HAVING关键字，用于对前面取到的集合
      SELECT COUNT(*), department_id 
      FROM employees 
      GROUP BY department_id;
      再次进行筛选】


      查询每个班级参加了运动会的同学的最高得分大于5分的姓名和最高分  #这里score为0的视为没有参加
      SELECT MAX(score), name
      FROM stu
      WHERE score IS NOT NULL
      GROUP BY class_id
      HAVING MAX(score) > 5;


      分组前筛选  使用的是原始表，只能使用原始表中的字段   使用  WHERE
      分组后筛选  使用分组之后的结果集 只能使用结果集中的字段 使用 HAVING
      优先考虑分组前筛选

      查询每个学院每个班级同学的平均年龄
      SELECT AVG(age) 平均年龄, class_id, college_id
      FROM stu
      GROUP BY class_id, college_id;
```
># 连接查询

>##  分类 
    按年代分类
      sql192标准 只支持内连接
      sql199标准 【推荐】mysql中 支持内连接+外连接（左右外连接）+交叉连接

    按功能分类
      内连接
          等值连接
          非等值连接
          自连接
      外连接
          坐外连接
          右外连接
          全外连接
      交叉连接
>## 等值连接

```SQL
  SELECT NAME, ADDRESS
  FROM stu, address
  WHERE address_id = address.id;

  --简单的懒得写了
  --要用到去查
```



># 子查询

>## 子查询分类
    按照子查询出现的位置
        SELECT后面
            仅仅支持标量子查询

        FROM后面
            支持表子查询
        WHERE或者HAVING后面★
            标量子查询（单行）√
            列子查询（多行）√
            行子查询

        EXISTS后面（相关子查询）
            表子查询
    按照结果集的行列数不同
        标量子查询（结果集只有一行一列）
        列子查询（结果集只有一列多行）
        行子查询（结果一行多列）
        表子查询（结果集一般为多行多列）

### where或者having后面
#### 特点
  1. 子查询放在小括号内  
  2. 子查询一般放在条件的右侧  
  3. 标量子查询，一般搭配着单行操作符使用  
      > \> < >= <= = <>
  4. 列子查询，一般搭配多行操作符使用  
      > IN、ANY/SOME、ALL

    
#### 标量子查询（单行子查询）
  >谁的年龄比小明大 

  `SELECT * FROM stu WHERE age > (SELECT age FROM stu WHERE name = '小明');`

  >查询class_id与小明相同，年龄age比小王大的同学的姓名，年龄  
  
  `SELECT name, age FROM stu WHERE class_id = (SELECT class_id FROM stu WHERE name='小明') AND age > (SELECT age FROM stu WHERE name = '小王');`
  
  >查询年龄最小的同学的所有信息

  `SELECT * FROM stu WHERE age = (SELECT MIN(age) FROM stu);`

  >查询最小年龄大于网工班（class_id为100100）的最小年龄的班级id和其最小年龄

  ```SQL
    --1.查询班级id为100100最小年龄
    SELECT MIN(age) 
    FROM stu 
    WHERE class_id = '100100'

    --2.查询每个班级的最小年龄
    SELECT MIN(age) 
    FROM stu 
    GROUP BY class_id;

    --3.筛选
    SELECT MIN(age) 
    FROM stu 
    GROUP BY class_id
    HAVING MIN(age) > (
      SELECT MIN(age)
      FROM stu
      WHERE class_id = '100100'
    );
    --子查询的执行优先于主查询
  ``` 
  >示例1  

  `SELECT * FROM stu WHERE id IN (SELECT id FROM stu WHRE age > 20)`
>示例2

  `SELECT * FROM stu WHERE age < ANY(SELECT age FROM stu WHERE class_id='100010') AND class_id <> '100010';--个人感觉这个any没啥用啊` 
>示例3

  `SELECT * FROM stu WHERE age < ALL(SELECT age FROM stu WHERE class_id='100010') AND class_id <> '100010';`

#### 列子查询（多行子查询）
```sql
  --查询学号最小年龄最大的学生信息
  --此类用的比较少
  SELECT *
  FROM stu
  WHERE (id, age) = (
    SELECT MIN(id), MAX(age)
    FROM stu
  );
```


#### SELECT后面子查询
>查询每个班级的学生数

```sql
  SELECT c.*, (
    SELECT COUNT(*)
    FROM stu s
    WHERE s.class_id = c.id
  ) 个数
  FROM class;
```
>查询学号为201011的班级名称

```sql
--自己写吧
```
#### from后面
>查询每个部门的平均年龄等级
```sql
employees:
  name  department_id  age
   张三      100001      22
   李四      100002      25
   王二麻子  100002      30
  ........................
age_grade:
  name  mix_age  max_age
   老年   60        100
   中年   40        60
   .............
  
  SELECT dep.* ,ag.name
  FROM (
    SELECT AVG(age) age, department
    FROM employees
    GROUP BY department_id
  ) dep
  INNER JOIN age_grade ag
  ON dep.age BETWEEN mix_age AND max_age;

  --也不知道对不对，就这样瞎鸡儿写
```

#### 行子查询相关子查询
```sql
  SELECT EXISTS(SELECT age FROM stu);

  --查询有员工的部门名
  SELECT department_name
  FROM departments
  WHERE EXISTS(
    SELECT id 
    FROM employees 
    WHERE employees.department_id = departments_department_id
  );

  --exit先执行外查询，再只查询筛选，能用exists一定能用in实现

```
## 联合查询
```sql
  --查询学生姓名包含  海 或者年龄大于21的学生信息
  --1.一般的查询
  SELECT * FROM stu WHERE stu.name LIKE '%王%' OR age > 21;

  --2.联合查询
  SELECT * FROM stu WHERE stu.name LIKE '%王%'
  UNION
  SELECT * FROM stu WHERE age > 21;

  --如果条件比较多，可以使用联合查询,或者有两张一样的表，需要查询信息的时候要用到联合查询； 关键字查询，需要查询很多张表

  --多条查询语句的查询列数必须是一致的，每一列的类型和顺序是一致的

  --UNION会去重  如果不需要去重可以使用  UNION ALL
```
