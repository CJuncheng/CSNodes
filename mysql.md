# MySQL Notes
<h2 align="left">目录</h2>
[toc]

## 数据库概述

**DB：数据库（Database）**
即存储数据的“仓库”，其本质是一个文件系统。它保存了一系列有组织的数据。

**DBMS：数据库管理系统（Database Management System）**
是一种操纵和管理数据库的大型软件，用于建立、使用和维护数据库，对数据库进行统一管理和控制。用户通过数据库管理系统访问数据库中表内的数据。

**SQL：结构化查询语言（Structured Query Language）**
专门用来与数据库通信的语言。

**表（table）** 某种特定类型数据的结构化清单。数据库每个表都有自己名字，是每个表的唯一标识号。

### 数据库管理系统
数据库管理系统(DBMS)可以管理多个数据库，一般开发人员会针对每一个应用创建一个数据库。为保存应用中实体的数据，一般会在数据库创建多个表，以保存程序中实体用户的数据。DBMS可分为两类：基于共享文件系统的DBMS和基于客户机—服务器的DBMS。后面主要介绍基于客户机—服务器的DBMS。数据库管理系统与数据库关系紧密，数据库管理系统、数据库和表的关系如图所示：

<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/DBMS.jpg"/>

目前互联网上常见的数据库管理软件有Oracle、MySQL、MS SQL Server、DB2、PostgreSQL、Access、Sybase、Informix这几种。

**Oracle vs MySQL**
- Oracle 更适合大型跨国企业的使用，因为他们对费用不敏感，但是对性能要求以及安全性有更高的要求。
- MySQL 由于其**体积小、速度快、总体拥有成本低，可处理上千万条记录的大型数据库，尤其是开放源码这一特点，使得很多互联网公司、中小型网站选择了MySQL作为网站数据库**（Facebook，Twitter，YouTube，阿里巴巴/蚂蚁金服，去哪儿，美团外卖，腾讯）。

#### 关系型数据库(RDBMS)
这种类型的数据库是**最古老**的数据库类型，关系型数据库模型是把复杂的数据结构归结为简单的**二元关系** （即二维表格形式）

关系型数据库以 **行(row) 和 列(column)** 的形式存储数据。如把顾客表想象为网格，一个列存储着顾客的编号，一个列存储顾客名；一个行存储一个顾客信息。
- **列**（column） 表中的一个字段, 用来存储表中部分信息。所有表都是由一个或多个列组成的
  - **数据类型**（datatype） 所容许的数据的类型。每个表列都有相
应的数据类型，它限制（或容许）该列中存储的数据。
- **行**（row）：表中的一个记录。
- **主键**（primary key）：一列（或一组列），其值能够唯一区分表中每个行。

**1. ER模型**
- E-R（entity-relationship，实体-联系）模型中有三个主要概念是： **实体集 、 属性 、 联系集**。
- 一个实体集（class）对应于数据库中的一个表（table），一个实体（instance）则对应于数据库表中的一行（row），也称为一条记录（record）。一个属性（attribute）对应于数据库表中的一列（column），也称为一个字段（field）。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/ER.png"/>

**2.  表的关联关系**
- 一对一关联
- 一对多关联：客户表和订单表 ， 分类表和商品表 ， 部门表和员工表 。
- 多对多关联：要表示多对多关系，必须创建第三个表，该表通常称为**联接表**。例如
  - 产品表 ：“产品”表中的每条记录表示一个产品。
  - 订单表 ：“订单”表中的每条记录表示一个订单。
  - 订单明细表 ：每个产品可以与“订单”表中的多条记录对应，即出现在多个订单中。一个订单可以与“产品”表中的多条记录对应，即包含多个产品。
- 自我引用

#### 非关系型数据库(RDBMS)
非关系型数据库，可看成传统关系型数据库的功能**阉割版本**，基于键值对存储数据，不需要经过SQL层的解析，**性能非常高**。同时，通过减少不常用的功能，进一步提高性能。
- **键值型数据库**： 键值型数据库典型的使用场景是作为**内存缓存**。 Redis 是最流行的键值型数据库。
- **文档型数据库**：MongoDB是最流行的文档型数据库。此外，还有CouchDB等。
- **搜索引擎数据库**：全文索引效率高。核心原理是“倒排索引”。典型产品：Solr、Elasticsearch、Splunk 等。
- **列式数据库**：大量降低系统的I/O，适合于分布式文件系统，不足在于功能相对有限。典型产品：HBase等。
- **图形数据库**：存储图形关系的数据库。典型产品：Neo4J、InfoGrid等。



### 结构化查询语言

SQL(Structured Query Language) 是一门 ANSI 的标准计算机语言，用来访问和操作数据库系统。存在着很多不同版本的 SQL 语言，但是为了与 ANSI 标准相兼容，它们必须以相似的方式共同地来支持一些主要的关键词（比如 SELECT、UPDATE、DELETE、INSERT、WHERE 等等）。

#### SQL 分类

SQL语言在功能上主要分为如下3大类：
- **DDL（Data Definition Languages、数据定义语言）**，这些语句定义了不同的数据库、表、视图、索引等数据库对象，还可以用来创建、删除、修改数据库和数据表的结构。
  - 主要的语句关键字包括**CREATE 、DROP 、ALTER** 等。
- **DML（Data Manipulation Language、数据操作语言）**，用于添加、删除、更新和查询数据库记录，并检查数据完整性。
  - 主要的语句关键字包括**INSERT 、DELETE 、UPDATE 、SELECT**等。
  - **SELECT是SQL语言的基础，最为重要**。
- **DCL（Data Control Language、数据控制语言）**，用于定义数据库、表、字段、用户的访问权限和安全级别。
  - 主要的语句关键字包括**GRANT 、REVOKE 、COMMIT、ROLLBACK 、SAVEPOINT**等。

> 因为查询语句使用的非常的频繁，所以很多人把查询语句单拎出来一类：DQL（数据查询语言）。还有单独将COMMIT 、ROLLBACK 取出来称为TCL （Transaction Control Language，事务控制语言）。

#### SQL语言的规则与规范
SQL的规则 ----必须要遵守
- SQL 可以写在一行或者多行。为了提高可读性，各子句分行写，必要时使用缩进
- 每条命令以 ; 或 \g 或 \G 结束
- 关键字不能被缩写也不能分行
- 关于标点符号
  - 必须保证所有的()、单引号、双引号是成对结束的
  - 必须使用英文状态下的半角输入方式
  - 字符串型和日期时间类型的数据可以使用单引号（' '）表示
  - 列的别名，尽量使用双引号（" "），而且不建议省略as
- 关于注释
  - 单行注释：``#注释文字(MySQL特有的方式)``
  - 单行注释：``-- 注释文字(--后面必须包含一个空格。)``
  - 多行注释：``/* 注释文字 */``

SQL的规范  ----建议遵守
- MySQL 在 Windows 环境下是大小写不敏感的
- MySQL 在 Linux 环境下是大小写敏感的
  - 数据库名、表名、表的别名、变量名是严格区分大小写的
  - 关键字、函数名、列名(或字段名)、列的别名(字段的别名) 是忽略大小写的。
- 推荐采用统一的书写规范：
  - 数据库名、表名、表别名、字段名、字段别名等都小写
  - SQL 关键字、函数名、绑定变量等都大写

## SQL之SELECT使用

### 基本的SELECT语句
```sql
#1. 最基本的SELECT语句： SELECT 字段1,字段2,... FROM 表名 
SELECT 1 + 1,3 * 2;
SELECT 1 + 1,3 * 2
FROM DUAL; #dual：伪表

# *:表中的所有的字段（或列）
SELECT * FROM employees;
SELECT employee_id,last_name,salary
FROM employees;


#2. 列的别名
# as:全称：alias(别名),可以省略
# 列的别名可以使用一对""引起来，不要使用''。字符串使用单引号
SELECT employee_id emp_id,last_name AS lname,department_id "部门id",salary * 12 AS "annual sal"
FROM employees;

#3. 去除重复行
#查询员工表中一共有哪些部门id呢？
#错误的:没有去重的情况
SELECT department_id
FROM employees;
#正确的：去重的情况
SELECT DISTINCT department_id
FROM employees;

#错误的：
SELECT salary,DISTINCT department_id
FROM employees;

#仅仅是没有报错，但是没有实际意义。
SELECT DISTINCT department_id,salary
FROM employees;

#4. 空值参与运算
# 1. 空值：null
# 2. null不等同于0，''，'null'
SELECT * FROM employees;

# 3. 空值参与运算：结果一定也为空。
SELECT employee_id,salary "月工资",salary * (1 + commission_pct) * 12 "年工资",commission_pct
FROM employees;
#实际问题的解决方案：引入IFNULL
SELECT employee_id,salary "月工资",salary * (1 + IFNULL(commission_pct,0)) * 12 "年工资",commission_pct
FROM `employees`;

#5. 着重号 ``
SELECT * FROM `order`; # order是关键字，数据库表名与关键字相同，需要使用着重号

#6. 查询常数
SELECT '尚硅谷',123,employee_id,last_name
FROM employees;

#7.显示表结构
DESCRIBE employees; #显示了表中字段的详细信息
DESC employees;

#8.过滤数据
#练习：查询90号部门的员工信息
SELECT * 
FROM employees
#过滤条件,声明在FROM结构的后面
WHERE department_id = 90;
#练习：查询last_name为'King'的员工信息
SELECT * 
FROM EMPLOYEES
WHERE LAST_NAME = 'King'; 
```

### 运算符
1. 算术运算符
  算术运算符： +  -  *  /(or div)  %(or mod)
    ```sql
    SELECT 100, 100 + 0, 100 - 0, 100 + 50, 100 + 50 * 30, 100 + 35.5, 100 - 35.5 
    FROM DUAL;

    # 在SQL中，+没有连接的作用，就表示加法运算。此时，会将字符串转换为数值（隐式转换）
    SELECT 100 + '1'  # 在Java语言中，结果是：1001。 
    FROM DUAL;

    SELECT 100 + 'a' #此时将'a'看做0处理
    FROM DUAL;

    SELECT 100 + NULL  # null值参与运算，结果为null
    FROM DUAL;

    SELECT 100, 100 * 1, 100 * 1.0, 100 / 1.0, 100 / 2,
    100 + 2 * 5 / 2,100 / 3, 100 DIV 0  # 分母如果为0，则结果为null
    FROM DUAL;

    # 取模运算： % mod
    SELECT 12 % 3,12 % 5, 12 MOD -5,-12 % 5,-12 % -5
    FROM DUAL;

    #练习：查询员工id为偶数的员工信息
    SELECT employee_id,last_name,salary
    FROM employees
    WHERE employee_id % 2 = 0;
    ```

2. 比较运算符
    - = \ <=> \ <>(!=) \ < \ <= \ > \ >= 
      ```sql
      # = 的使用
      SELECT 1 = NULL,NULL = NULL # 只要有null参与判断，结果就为null
      FROM DUAL;
      SELECT last_name,salary,commission_pct
      FROM employees
      #where salary = 6000;
      WHERE commission_pct = NULL;  #此时执行，不会有任何的结果

      # <=> ：安全等于。 记忆技巧：为NULL而生。
      #练习：查询表中commission_pct为null的数据有哪些
      SELECT last_name,salary,commission_pct
      FROM employees
      WHERE commission_pct <=> NULL;
      ```
    - IS NULL \ IS NOT NULL \ ISNULL
      ```sql
      #练习：查询表中commission_pct为null的数据有哪些
      SELECT last_name,salary,commission_pct
      FROM employees
      WHERE commission_pct IS NULL;
      #或
      SELECT last_name,salary,commission_pct
      FROM employees
      WHERE ISNULL(commission_pct);

      #练习：查询表中commission_pct不为null的数据有哪些
      SELECT last_name,salary,commission_pct
      FROM employees
      WHERE commission_pct IS NOT NULL;
      #或
      SELECT last_name,salary,commission_pct
      FROM employees
      WHERE NOT commission_pct <=> NULL;
      ```
    - LEAST() \ GREATEST()
      ```sql
      SELECT LEAST('g','b','t','m'),GREATEST('g','b','t','m')
      FROM DUAL;
      SELECT LEAST(first_name,last_name),LEAST(LENGTH(first_name),LENGTH(last_name))
      FROM employees;
      ``` 
    - BETWEEN 条件下界1 AND 条件上界2  （查询条件1和条件2范围内的数据，包含边界）
      ```sql
      #查询工资在6000 到 8000的员工信息
      SELECT employee_id,last_name,salary
      FROM employees
      #where salary between 6000 and 8000;
      WHERE salary >= 6000 && salary <= 8000;

      #查询工资不在6000 到 8000的员工信息
      SELECT employee_id,last_name,salary
      FROM employees
      WHERE salary NOT BETWEEN 6000 AND 8000;
      #where salary < 6000 or salary > 8000;
      ``` 
    - in (set)\ not in (set)
      ```sql
      #练习：查询部门为10,20,30部门的员工信息
      SELECT last_name,salary,department_id
      FROM employees
      #where department_id = 10 or department_id = 20 or department_id = 30;
      WHERE department_id IN (10,20,30);

      #练习：查询工资不是6000,7000,8000的员工信息
      SELECT last_name,salary,department_id
      FROM employees
      WHERE salary NOT IN (6000,7000,8000);
      ``` 
    - LIKE :模糊查询
      - % : 代表不确定个数的字符 （0个，1个，或多个）
      - _ ：代表一个不确定的字符
    - REGEXP \ RLIKE :正则表达式
      REGEXP运算符用来匹配字符串，语法格式为： ``expr REGEXP 匹配条件``。如果expr满足匹配条件，返回 
      - ‘^’匹配以该字符后面的字符开头的字符串。
      - ‘$’匹配以该字符前面的字符结尾的字符串。
      - ‘.’匹配任何一个单字符。
      - “[...]”匹配在方括号内的任何字符。例如，“[abc]”匹配“a”或“b”或“c”。为了命名字符的范围，使用一个‘-’。“[a-z]”匹配任何字母，而“[0-9]”匹配任何数字。
      - ‘\*’  匹配零个或多个在它前面的字符。例如，“x*”匹配任何数量的‘x’字符，“[0-9]*”匹配任何数量的数字，而“*”匹配任何数量的任何字符。
     
      ```sql
      SELECT 'shkstart' REGEXP '^shk', 'shkstart' REGEXP 't$', 'shkstart' REGEXP 'hk'
      FROM DUAL;
      ``` 

3. 逻辑运算符
  逻辑运算符： OR(or ||)  AND(or &&)  NOT(or !)  XOR
    ```sql
    # or  and 
    SELECT last_name,salary,department_id
    FROM employees
    #where department_id = 10 or department_id = 20;
    #where department_id = 10 and department_id = 20;
    WHERE department_id = 50 AND salary > 6000;

    # not 
    SELECT last_name,salary,department_id
    FROM employees
    #where salary not between 6000 and 8000;
    #where commission_pct is not null;
    WHERE NOT commission_pct <=> NULL;

    # XOR :追求的"异"
    SELECT last_name,salary,department_id
    FROM employees
    WHERE department_id = 50 XOR salary > 6000;
    ```
4. 位运算符
  位运算符： & |  ^  ~  >>   <<
    ```sql
    SELECT 12 & 5, 12 | 5,12 ^ 5 
    FROM DUAL;
    SELECT 10 & ~1 FROM DUAL;

    #在一定范围内满足：每向左移动1位，相当于乘以2；每向右移动一位，相当于除以2。
    SELECT 4 << 1 , 8 >> 1
    FROM DUAL;
    ```

### 排序与分页
#### 排序

使用 ORDER BY 对查询到的数据进行排序操作。
- 升序：ASC (ascend) 默认升序排序
- 降序：DESC (descend)
  
```sql
# 练习：按照salary从高到低的顺序显示员工信息
SELECT employee_id,last_name,salary
FROM employees
ORDER BY salary DESC;
```
> 可以使用列的别名，进行排序。列的别名只能在 ORDER BY 中使用，不能在WHERE中使用。

强调格式：WHERE 需要声明在FROM后，ORDER BY之前。
```sql
SELECT employee_id,salary, department_id
FROM employees
WHERE department_id IN (50,60,70)
ORDER BY department_id DESC;
```

多列排序：在对多列进行排序的时候，**首先排序的第一列必须有相同的列值，才会对第二列进行排序。如果第一列数据中所有值都是唯一的，将不再对第二列进行排序**。
```sql
#练习：显示员工信息，按照department_id的降序排列；department_id相同时，按salary的升序排列
SELECT employee_id,salary,department_id
FROM employees
ORDER BY department_id DESC,salary ASC;
```

#### 分页
查询返回的记录太多了，实现分页查询请求，前面页完了，再请求接下来的页。mysql使用limit实现数据的分页显示。
```
LIMIT [位置偏移量,] 行数
```
```sql
#需求：每页显示pageSize条记录，此时显示第pageNo页：
#公式：LIMIT (pageNo-1) * pageSize,pageSize;
# 需求：每页显示20条记录，此时显示第3页
SELECT employee_id,last_name
FROM employees
LIMIT 40,20;

#WHERE ... ORDER BY ...LIMIT 声明顺序如下：
# LIMIT的格式： 严格来说：LIMIT 位置偏移量,条目数
# 结构"LIMIT 0,条目数" 等价于 "LIMIT 条目数"
SELECT employee_id,last_name,salary
FROM employees
WHERE salary > 6000
ORDER BY salary DESC
#limit 0,10;
LIMIT 10;
```

### 多表查询(重要)

多表查询：也称为关联查询，指两个或更多个表一起完成查询操作。
前提条件：这些一起查询的表之间是有关系的（一对一、一对多），它们之间一定是有关联字段，这个关联字段可能建立了外键，也可能没有建立外键。比如：员工表和部门表，这两个表依靠“部门编号”进行关联。


笛卡尔积的错误: 组合的个数即为两个集合中元素个数的乘积数。
笛卡尔积的错误会在下面条件下产生：
- 省略多个表的连接条件（或关联条件）
- 连接条件（或关联条件）无效
- 所有表中的所有行互相连接

为了避免笛卡尔积， 可以在 WHERE 加入有效的连接条件。
```sql
#多表查询的正确方式：需要有连接条件
SELECT employee_id,department_name
FROM employees,departments
#两个表的连接条件
WHERE employees.`department_id` = departments.department_id;
```

**注意**：
- 如果查询语句中出现了多个表中都存在的字段，则必须指明此字段所在的表。
- 使用FROM给表起别名(**FROM 后面语句最先执行**)，如果给表起了别名，一旦在SELECT或WHERE中使用表名的话，则必须使用表的别名，而不能再使用表的原名。
- 如果有n个表实现多表的查询，则需要至少n-1个连接条件。
```sql
#查询员工的employee_id,last_name,department_name,city
SELECT e.employee_id,e.last_name,d.department_name,l.city,e.department_id,l.location_id
FROM employees e,departments d,locations l
WHERE e.`department_id` = d.`department_id`
AND d.`location_id` = l.`location_id`;
```
#### 多表查询的分类
**等值连接 VS 非等值连接**
之前遇到的都是等值连接，举个非等值连接的例子
```sql
SELECT e.last_name,e.salary,j.grade_level
FROM employees e,job_grades j
#where e.`salary` between j.`lowest_sal` and j.`highest_sal`;
WHERE e.`salary` >= j.`lowest_sal` AND e.`salary` <= j.`highest_sal`;
```

**自连接 VS 非自连接**
之前遇到是非自连接的，举个自连接的例子
```sql
#练习：查询员工id,员工姓名及其管理者的id和姓名
SELECT emp.employee_id,emp.last_name,mgr.employee_id,mgr.last_name
FROM employees emp ,employees mgr
WHERE emp.`manager_id` = mgr.`employee_id`;
```

**内连接 VS 外连接**
- **内连接**：合并具有同一列的两个以上的表的行, 结果集中不包含一个表与另一个表不匹配的行。
- **外连接**：合并具有同一列的两个以上的表的行, 结果集中除了包含一个表与另一个表匹配的行之外，还查询到了左表 或 右表中不匹配的行。外连接的分类：左外连接、右外连接、满外连接。
  - **左外连接**：两个表在连接过程中除了返回满足连接条件的行以外还返回左表中不满足条件的行，这种连接称为左外连接。
  - **右外连接**：两个表在连接过程中除了返回满足连接条件的行以外还返回右表中不满足条件的行，这种连接称为右外连接。
  - **满外连接**：左外连接与右外连接的并

- SQL92语法实现内连接：见上，略
- SQL92语法实现外连接：使用 +(MySQL不支持SQL92语法中外连接的写法！)
- SQL99语法中使用 JOIN ...ON 的方式实现多表的查询。这种方式也能解决外连接的问题。MySQL是支持此种方式的。
  - 内连接：``INNER``可以省略
    ```sql
    SELECT last_name,department_name
    FROM employees e INNER JOIN departments d
    ON e.`department_id` = d.`department_id`;
    ``` 
  - 左外连接：``OUTER``可以省略
    ```sql
    SELECT last_name,department_name
    FROM employees e INNER OUTER JOIN departments d
    ON e.`department_id` = d.`department_id`;
    ``` 
  - 右外连接：同理
  - 满外连接：mysql不支持FULL OUTER JOIN
    > 使用UNION  和 UNION ALL实现

#### SQL JOINS
**UNION  和 UNION ALL的使用**
- UNION：操作符返回两个查询的结果集的并集，去除重复记录。会执行去重操作(效率低)
- UNION ALL: 操作符返回两个查询的结果集的并集。对于两个结果集的重复部分，不会执行去重操作。
> 则尽量使用UNION ALL语句，以提高数据查询的效率。

**7中 SQL JOINS**
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/SQL_JOIN.png"/>

```sql
# 中图：内连接(INNER JOIN)
SELECT employee_id,department_name
FROM employees e JOIN departments d
ON e.`department_id` = d.`department_id`;

# 左上图：左外连接(LEFT INCLUSIVE)
SELECT employee_id,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`;

# 右上图：右外连接(RIGHT INCLUSIVE)
SELECT employee_id,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`;

# 左中图：左内连接(LEFT EXCLUSIVE)
SELECT employee_id,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL;

# 右中图：右内连接(RIGHT EXCLUSIVE)
SELECT employee_id,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL;

# 左下图：满外连接
# 方式1：左上图 UNION ALL 右中图
SELECT employee_id,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
UNION ALL
SELECT employee_id,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL;

# 方式2：左中图 UNION ALL 右上图
SELECT employee_id,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
UNION ALL
SELECT employee_id,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`;

# 右下图：左中图  UNION ALL 右中图
SELECT employee_id,department_name
FROM employees e LEFT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE d.`department_id` IS NULL
UNION ALL
SELECT employee_id,department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL;
```
### 函数
DBMS 之间的差异性很大，远大于同一个语言不同版本之间的差异。大部分 DBMS 会有自己特定的函数，这就意味着采用 SQL 函数的代码可移植性是很差的，因此在使用函数的时候需要特别注意。

MySQL提供了丰富的内置函数，MySQL提供的内置函数从实现的功能角度可以分为数值函数、字符串函数、日期和时间函数、流程控制函数、加密与解密函数、获取MySQL信息函数、聚合函数等。这里，我将这些丰富的内置函数再分为两类： **单行函数**和**聚合函数**（或分组函数）。

<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/SQL_func.png"/>

#### 单行函数

单行函数
- 操作数据对象
- 接受参数返回一个结果
- **只对一行进行变换**
- **每行返回一个结果**
- 可以嵌套
- 参数可以是一列或一个值


1. 数值函数

|函数    | 用法    | 
|:----    | :---    |  
|ABS(x) | 返回x的绝对值 |
|SIGN(X) | 返回X的符号。正数返回1，负数返回-1，0返回0 |
|PI() |返回圆周率的值|
|CEIL(x)，CEILING(x)| 返回大于或等于某个值的最小整数|
|FLOOR(x) |返回小于或等于某个值的最大整数|
|LEAST(e1,e2,e3…) |返回列表中的最小值|
|GREATEST(e1,e2,e3…) |返回列表中的最大值|
|MOD(x,y) |返回X除以Y后的余数|
|RAND()| 返回0~1的随机值|
|RAND(x)|返回0~1的随机值，其中x的值用作种子值，相同的X值会产生相同的随机数|
|ROUND(x) |返回一个对x的值进行四舍五入后，最接近于X的整数|
|ROUND(x,y)| 返回一个对x的值进行四舍五入后最接近X的值，并保留到小数点后面Y位|
|TRUNCATE(x,y) |返回数字x截断为y位小数的结果|
|SQRT(x) |返回x的平方根。当X的值为负数时，返回NULL|

2. 字符串函数

|函数    | 用法    | 
|:----    | :---    |  
|ASCII(S) |返回字符串S中的第一个字符的ASCII码值|
|CHAR_LENGTH(s) |返回字符串s的字符数。作用与CHARACTER_LENGTH(s)相同|
|LENGTH(s) |返回字符串s的字节数，和字符集有关|
|CONCAT(s1,s2,......,sn) |连接s1,s2,......,sn为一个字符串|
|CONCAT_WS(x,s1,s2,......,sn) | 同CONCAT(s1,s2,...)函数，但是每个字符串之间要加上x|
|INSERT(str, idx, len,replacestr)|将字符串str从第idx位置开始，len个字符长的子串替换为字符串replacestr|
|REPLACE(str, a, b) | 用字符串b替换字符串str中所有出现的字符串a|
|UPPER(s) 或 UCASE(s) | 将字符串s的所有字母转成大写字母|
|LOWER(s) 或LCASE(s) | 将字符串s的所有字母转成小写字母|
|LEFT(str,n) | 返回字符串str最左边的n个字符|
|RIGHT(str,n) | 返回字符串str最右边的n个字符|
|LPAD(str, len, pad) | 用字符串pad对str最左边进行填充，直到str的长度为len个字符|
|RPAD(str ,len, pad) | 用字符串pad对str最右边进行填充，直到str的长度为len个字符|
|LTRIM(s) | 去掉字符串s左侧的空格|
|RTRIM(s) | 去掉字符串s右侧的空格|
|TRIM(s) | 去掉字符串s开始与结尾的空格|
|TRIM(s1 FROM s) | 去掉字符串s开始与结尾的s1|
|TRIM(LEADING s1 FROM s) | 去掉字符串s开始处的s1|
|TRIM(TRAILING s1 FROM s) | 去掉字符串s结尾处的s1|
|REPEAT(str, n) | 返回str重复n次的结果|
|SPACE(n) | 返回n个空格|
|STRCMP(s1,s2) | 比较字符串s1,s2的ASCII码值的大小|
|SUBSTR(s,index,len) | 返回从字符串s的index位置其len个字符，作用与SUBSTRING(s,n,len)、MID(s,n,len)相同|
|LOCATE(substr,str) | 返回字符串substr在字符串str中首次出现的位置，作用于POSITION(substr IN str)、INSTR(str,substr)相同。未找到，返回0|
|ELT(m,s1,s2,…,sn) | 返回指定位置的字符串，如果m=1，则返回s1，如果m=2，则返回s2，如果m=n，则返回sn|
|FIELD(s,s1,s2,…,sn)|返回字符串s在字符串列表中第一次出现的位置|
|FIND_IN_SET(s1,s2)|返回字符串s1在字符串s2中出现的位置。其中，字符串s2是一个以逗号分隔的字符串|
|REVERSE(s) | 返回s反转后的字符串|
|NULLIF(value1,value2)|比较两个字符串，如果value1与value2相等，则返回NULL，否则返回value1|

3. 日期和时间函数
- 获取日期、时间

|函数    | 用法    | 
|:----    | :---    |  
|CURDATE() ，CURRENT_DATE() | 返回当前日期，只包含年、月、日|
|CURTIME() ， CURRENT_TIME() | 返回当前时间，只包含时、分、秒|
|NOW() / SYSDATE() / CURRENT_TIMESTAMP() / LOCALTIME() / LOCALTIMESTAMP() |返回当前系统日期和时间|
|UTC_DATE() | 返回UTC（世界标准时间）日期|
|UTC_TIME() | 返回UTC（世界标准时间）时间|

- 日期与时间戳的转换

|函数    | 用法    | 
|:----    | :---    |  
|UNIX_TIMESTAMP() | 以UNIX时间戳的形式返回当前时间。SELECT UNIX_TIMESTAMP() ->1634348884|
|UNIX_TIMESTAMP(date) | 将时间date以UNIX时间戳的形式返回。|
|FROM_UNIXTIME(timestamp) | 将UNIX时间戳的时间转换为普通格式的时间|

- 获取月份、星期、星期数、天数等函数

|函数    | 用法    | 
|:----    | :---    |  
|YEAR(date) / MONTH(date) / DAY(date)  | 返回具体的日期值|
|HOUR(time) / MINUTE(time) / SECOND(time) | 返回具体的时间值|
|MONTHNAME(date) | 返回月份：January，...|
|DAYNAME(date) | 返回星期几：MONDAY，TUESDAY.....SUNDAY|
|WEEKDAY(date) | 返回周几，注意，周1是0，周2是1，。。。周日是6|
|QUARTER(date) | 返回日期对应的季度，范围为1～4|
|WEEK(date) ， WEEKOFYEAR(date) | 返回一年中的第几周|
|DAYOFYEAR(date) | 返回日期是一年中的第几天|
|DAYOFMONTH(date) | 返回日期位于所在月份的第几天|
|DAYOFWEEK(date) | 返回周几，注意：周日是1，周一是2，。。。周六是7|

- 日期的操作函数

|函数    | 用法    | 
|:----    | :---    |  
|EXTRACT(type FROM date)  | 返回指定日期中特定的部分，type指定返回的值|

EXTRACT(type FROM date)函数中type的取值与含义：


- 时间和秒钟转换的函数

- 计算日期和时间的函数

- 日期的格式化与解析
  


4. 流程控制函数

|函数    | 用法    | 
|:----    | :---    | 
|IF(value,value1,value2) |如果value的值为TRUE，返回value1，否则返回value2|
|IFNULL(value1, value2) |如果value1不为NULL，返回value1，否则返回value2|
|CASE WHEN 条件1 THEN 结果1 WHEN 条件2 THEN 结果2.... [ELSE resultn] END | 相当于Java的if...else if...else...|
|CASE expr WHEN 常量值1 THEN 值1 WHEN 常量值1 THEN 值1 .... [ELSE 值n] END | 相当于Java的switch...case...|

5. 加密和解密函数

|函数    | 用法    | 
|:----    | :---    | 
| PASSWORD(str) | 返回字符串str的加密版本，41位长的字符串。加密结果不可逆，常用于用户的密码加密 |
| MD5(str) | 返回字符串str的md5加密后的值，也是一种加密方式。若参数为NULL，则会返回NULL |
| SHA(str) | 从原明文密码str计算并返回加密后的密码字符串，当参数为NULL时，返回NULL。SHA加密算法比MD5更加安全。|
| ENCODE(value,password_seed) | 返回使用password_seed作为加密密码加密value |
| DECODE(value,password_seed) | 返回使用password_seed作为加密密码解密value |

6. MySQL信息函数

|函数    | 用法    | 
|:----    | :---    | 
|VERSION()  | 返回当前MySQL的版本号|
|CONNECTION_ID() | 返回当前MySQL服务器的连接数|
|DATABASE()，SCHEMA() |  返回MySQL命令行当前所在的数据库|
|USER()，CURRENT_USER()、SYSTEM_USER()，SESSION_USER() | 返回当前连接MySQL的用户名，返回结果格式为“主机名@用户名”|
|CHARSET(value) | 返回字符串value自变量的字符集|
|COLLATION(value) | 返回字符串value的比较规则|

#### 聚合函数
聚合函数作用于一组数据，并对一组数据返回一个值。聚合函数不能嵌套调用。

1. 常见的几个聚合函数
- AVG / SUM ：只适用于数值类型的字段（或变量）
  ```sql
  SELECT AVG(salary),SUM(salary),AVG(salary) * 107
  FROM employees;
  ``` 
- MAX / MIN :适用于数值类型、字符串类型、日期时间类型的字段（或变量）
  ```sql
  SELECT MAX(salary),MIN(salary)
  FROM employees;
  ``` 
- COUNT：
  - COUNT(*)返回表中记录总数，适用于任意数据类型。
  - COUNT(expr) 返回expr不为空的记录总数。
  - 如何需要统计表中的记录数，使用** COUNT(*)、COUNT(1)、COUNT(具体字段)哪个效率更高呢？
    - 如果使用的是MyISAM 存储引擎，则三者效率相同，都是O(1)
    - 如果使用的是InnoDB 存储引擎，则三者效率：COUNT(*) = COUNT(1)> COUNT(字段)
      > count(字段)<count(主键id)<count(1)≈count(*)
2. GROUP BY 的使用
可以使用GROUP BY子句将表中的数据分成若干组
```sql
SELECT department_id,AVG(salary),SUM(salary)
FROM employees
GROUP BY department_id
``` 
> - SELECT中出现的非组函数的字段必须声明在GROUP BY 中。反之，GROUP BY中声明的字段可以不出现在SELECT中。
> - GROUP BY 声明在FROM后面、WHERE后面，ORDER BY 前面、LIMIT前面

MySQL中GROUP BY中使用WITH ROLLUP，增加一条记录，如下例子，计算整体薪资均值
```sql
SELECT department_id,AVG(salary)
FROM employees
GROUP BY department_id WITH ROLLUP;
``` 
> 当使用ROLLUP时，不能同时使用ORDER BY子句进行结果排序，即ROLLUP和ORDER BY是互相排斥的。

3. HAVING的使用 (作用：用来过滤数据的)

要求：
- 如果过滤条件中使用了聚合函数，则必须使用HAVING来替换WHERE。否则，报错。
- HAVING 必须声明在 GROUP BY 的后面。
- 开发中，我们使用HAVING的前提是SQL中使用了GROUP BY。
> 当过滤条件中有聚合函数时, 过滤条件必须声明在HAVING中; 当过滤条件中没有聚合函数时，建议声明在WHERE中（效率更高）。
```sql
#练习：查询部门id为10,20,30,40这4个部门中最高工资比10000高的部门信息
#方式1：推荐，执行效率高于方式2.
SELECT department_id,MAX(salary)
FROM employees
WHERE department_id IN (10,20,30,40)
GROUP BY department_id
HAVING MAX(salary) > 10000;
``` 

WHERE 与 HAVING 的对比
- 从适用范围上来讲，HAVING的适用范围更广。 
- 如果过滤条件中没有聚合函数：这种情况下，WHERE的执行效率要高于HAVING

4. SQL底层执行原理

SELECT 语句的完整结构
```sql
#sql92语法：
SELECT ....,....,....(存在聚合函数)
FROM ...,....,....
WHERE 多表的连接条件 AND 不包含聚合函数的过滤条件
GROUP BY ...,....
HAVING 包含聚合函数的过滤条件
ORDER BY ....,...(ASC / DESC )
LIMIT ...,....

#sql99语法：
SELECT ....,....,....(存在聚合函数)
FROM ... (LEFT / RIGHT)JOIN ....ON 多表的连接条件 
(LEFT / RIGHT)JOIN ... ON ....
WHERE 不包含聚合函数的过滤条件
GROUP BY ...,....
HAVING 包含聚合函数的过滤条件
ORDER BY ....,...(ASC / DESC ) 排序
LIMIT ...,.... 分页
``` 
SELECT 查询关键字顺序如上所示，**SELECT 语句的执行顺序**如下：
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/SELECT_execution_sequence.png"/>

```
FROM ...,...-> ON -> (LEFT/RIGNT  JOIN) -> WHERE -> GROUP BY -> HAVING -> 
SELECT -> 
DISTINCT -> ORDER BY -> LIMIT
```

### 子查询(重要)
子查询指一个查询语句嵌套在另一个查询语句内部的查询。

谁的工资比Abel的高？
```sql
#方式1：自连接
SELECT e2.last_name,e2.salary
FROM employees e1,employees e2
WHERE e2.`salary` > e1.`salary` #多表的连接条件
AND e1.last_name = 'Abel';

#方式2：子查询
SELECT last_name,salary
FROM employees
WHERE salary > (
		SELECT salary
		FROM employees
		WHERE last_name = 'Abel'
		);
```

称谓的规范：外查询（或主查询）、内查询（或子查询）
- 子查询（内查询）在主查询之前一次执行完成。
- 子查询的结果被主查询（外查询）使用 。
- 注意事项
  - 子查询要包含在括号内
  - 将子查询放在比较条件的右侧
  - 单行操作符对应单行子查询，多行操作符对应多行子查询

**子查询的分类**
- 角度1：从内查询返回的结果的条目数: 单行子查询  vs  多行子查询
- 角度2：内查询是否被执行多次: 相关子查询  vs  不相关子查询
  - 相关子查询: 子查询从数据表中查询了数据结果，如果这个数据结果只执行一次，然后这个数据结果作为主查询的条件进行执行，那么这样的子查询叫做不相关子查询。
  - 如果子查询需要执行多次，即采用循环的方式，先从外部查询开始，每次都传入子查询进行查询，然后再将结果反馈给外部，这种嵌套的执行方式就称为相关子查询。

#### 单行子查询
单行操作符： =,  !=(<>),   >,    >=,   <,   <=, 

```sql
#题目：查询与141号员工的manager_id和department_id相同的其他员工
#的employee_id，manager_id，department_id。
#方式1：
SELECT employee_id,manager_id,department_id
FROM employees
WHERE manager_id = (
		    SELECT manager_id
		    FROM employees
		    WHERE employee_id = 141
		   )
AND department_id = (
		    SELECT department_id
		    FROM employees
		    WHERE employee_id = 141
		   )
AND employee_id <> 141;
```
#### 多行子查询

多行子查询的操作符： IN  ANY ALL SOME(同ANY)
|操作符    | 含义   | 
|:----    | :---    | 
|IN |等于列表中的任意一个|
|ANY | 需要和单行比较操作符一起使用，和子查询返回的**某一个值**比较|
|ALL |需要和单行比较操作符一起使用，和子查询返回的**所有值**比较|

```sql
#题目：查询平均工资最低的部门id
#MySQL中聚合函数是不能嵌套使用的。
#方式1：
SELECT department_id
FROM employees
GROUP BY department_id
HAVING AVG(salary) = (
			SELECT MIN(avg_sal)
			FROM(
				SELECT AVG(salary) avg_sal
				FROM employees
				GROUP BY department_id
				) t_dept_avg_sal
			);

#方式2：
SELECT department_id
FROM employees
GROUP BY department_id
HAVING AVG(salary) <= ALL(	
			SELECT AVG(salary) avg_sal
			FROM employees
			GROUP BY department_id
			) 
```

#### 相关子查询
前面介绍的都是不相关子查询。
相关子查询按照一行接一行的顺序执行，主查询的每一行都执行一次子查询。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/select_correlated_subquery.png"/>

```sql
#题目：查询员工中工资大于本部门平均工资的员工的last_name,salary和其department_id
#方式1：使用相关子查询
SELECT last_name,salary,department_id
FROM employees e1
WHERE salary > (
		SELECT AVG(salary)
		FROM employees e2
		WHERE department_id = e1.`department_id`
		);
  
#方式2：在FROM中声明子查询
SELECT e.last_name,e.salary,e.department_id
FROM employees e,(
		SELECT department_id,AVG(salary) avg_sal
		FROM employees
		GROUP BY department_id) t_dept_avg_sal
WHERE e.department_id = t_dept_avg_sal.department_id
AND e.salary > t_dept_avg_sal.avg_sal
```
> 在SELECT中，除了GROUP BY 和 LIMIT之外，其他位置都可以声明子查询！

**EXISTS 与 NOT EXISTS关键字**
- 关联子查询通常也会和 EXISTS操作符一起来使用，用来检查在子查询中是否存在满足条件的行。
  - 如果在子查询中不存在满足条件的行：
    - 条件返回 FALSE
    - 继续在子查询中查找
  - 如果在子查询中存在满足条件的行：
    - 不在子查询中继续查找
    - 条件返回 TRUE
- NOT EXISTS关键字表示如果不存在某种条件，则返回TRUE，否则返回FALSE。

```sql
#题目：查询公司管理者的employee_id，last_name，job_id，department_id信息
#方式1：自连接
SELECT DISTINCT mgr.employee_id,mgr.last_name,mgr.job_id,mgr.department_id
FROM employees emp JOIN employees mgr
ON emp.manager_id = mgr.employee_id;

#方式2：子查询
SELECT employee_id,last_name,job_id,department_id
FROM employees
WHERE employee_id IN (
			SELECT DISTINCT manager_id
			FROM employees
			);

#方式3：使用EXISTS
SELECT employee_id,last_name,job_id,department_id
FROM employees e1
WHERE EXISTS (
	       SELECT *
	       FROM employees e2
	       WHERE e1.`employee_id` = e2.`manager_id`
	     );
```

```sql
#题目：查询departments表中，不存在于employees表中的部门的department_id和department_name
#方式1：
SELECT d.department_id,d.department_name
FROM employees e RIGHT JOIN departments d
ON e.`department_id` = d.`department_id`
WHERE e.`department_id` IS NULL;

#方式2：
SELECT department_id,department_name
FROM departments d
WHERE NOT EXISTS (
		SELECT *
		FROM employees e
		WHERE d.`department_id` = e.`department_id`
		);
```

## SQL之DDL、DML、DCL使用

- **DDL（Data Definition Languages、数据定义语言）**，这些语句定义了不同的数据库、表、视图、索引等数据库对象，还可以用来创建、删除、修改数据库和数据表的结构。
  - 主要的语句关键字包括**CREATE 、DROP 、ALTER** 等。
- **DML（Data Manipulation Language、数据操作语言）**，用于添加、删除、更新和查询数据库记录，并检查数据完整性。
  - 主要的语句关键字包括**INSERT 、DELETE 、UPDATE 、SELECT**等。
  - **SELECT是SQL语言的基础，最为重要**。
- **DCL（Data Control Language、数据控制语言）**，用于定义数据库、表、字段、用户的访问权限和安全级别。
  - 主要的语句关键字包括**GRANT 、REVOKE 、COMMIT、ROLLBACK 、SAVEPOINT**等。


### 创建和管理表

1. 创建和管理数据库
- 创建数据库
  ```sql
  #方式1：
  CREATE DATABASE mytest1;  # 创建的此数据库使用的是默认的字符集
  #查看创建数据库的结构
  SHOW CREATE DATABASE mytest1;
  #方式2：显式了指名了要创建的数据库的字符集
  CREATE DATABASE mytest2 CHARACTER SET 'gbk';
  #
  SHOW CREATE DATABASE mytest2;
  #方式3（推荐）：如果要创建的数据库已经存在，则创建不成功，但不会报错。
  CREATE DATABASE IF NOT EXISTS mytest2 CHARACTER SET 'utf8';
  SHOW DATABASES;
  ```
- 管理数据库
  ```sql
  #查看当前连接中的数据库都有哪些
  SHOW DATABASES;
  #切换数据库
  USE atguigudb;
  #查看当前数据库中保存的数据表
  SHOW TABLES;
  #查看当前使用的数据库
  SELECT DATABASE() FROM DUAL;
  #查看指定数据库下保存的数据表
  SHOW TABLES FROM mysql;
  ```
- 修改数据库
  ```sql
  #更改数据库字符集
  SHOW CREATE DATABASE mytest2;
  ALTER DATABASE mytest2 CHARACTER SET 'utf8';
  #轻易不改数据库，注意：DATABASE不能改名。一些可视化工具可以改名，它是建新库，把所有表复制到新库，再删旧库完成的。
  ```
- 删除数据库
  ```sql
  #方式1：如果要删除的数据库存在，则删除成功。如果不存在，则报错
  DROP DATABASE mytest1;
  #方式2：推荐。 如果要删除的数据库存在，则删除成功。如果不存在，则默默结束，不会报错。
  DROP DATABASE IF EXISTS mytest1;
  ```

2. 创建数据表
- 方式1："白手起家"的方式
  ```sql
  CREATE TABLE IF NOT EXISTS myemp1(   #需要用户具备创建表的权限。
  id INT,
  emp_name VARCHAR(15), #使用VARCHAR来定义字符串，必须在使用VARCHAR时指明其长度。
  hire_date DATE
  );
  #查看表结构
  DESC myemp1;
  #查看创建表的语句结构
  SHOW CREATE TABLE myemp1; #如果创建表时没有指明使用的字符集，则默认使用表所在的数据库的字符集。
  #查看表数据
  SELECT * FROM myemp1;
  ```
- 方式2：基于现有的表，同时导入数据
  ```sql
  CREATE TABLE myemp2
  AS
  SELECT employee_id,last_name,salary
  FROM employees;
  DESC myemp2;
  SELECT *
  FROM myemp2;

  #说明1：查询语句中字段的别名，可以作为新创建的表的字段的名称。
  #说明2：此时的查询语句可以结构比较丰富，使用前面章节讲过的各种SELECT
  CREATE TABLE myemp3
  AS
  SELECT e.employee_id emp_id,e.last_name lname,d.department_name
  FROM employees e JOIN departments d
  ON e.department_id = d.department_id;
  SELECT *
  FROM myemp3;
  ```
3. 修改表
- 添加一个字段
  ```sql
  ALTER TABLE myemp1
  ADD salary DOUBLE(10,2); #默认添加到表中的最后一个字段的位置（8， 2）

  ALTER TABLE myemp1
  ADD phone_number VARCHAR(20) FIRST;

  ALTER TABLE myemp1
  ADD email VARCHAR(45) AFTER emp_name;
  ```
- 修改一个字段：数据类型、长度、默认值（略）
  ```sql
  ALTER TABLE myemp1
  MODIFY emp_name VARCHAR(25) ;

  ALTER TABLE myemp1
  MODIFY emp_name VARCHAR(35) DEFAULT 'aaa';
  ```
- 重命名一个字段
  ```sql
  ALTER TABLE myemp1
  CHANGE salary monthly_salary DOUBLE(10,2);

  ALTER TABLE myemp1
  CHANGE email my_email VARCHAR(50);
  ```
- 删除一个字段
  ```sql
  ALTER TABLE myemp1
  DROP COLUMN my_email;
  ```
4. 重命名表
    ```sql
    RENAME TABLE myemp1
    TO myemp11;
    ```
5. 删除表
    ```sql
    #不光将表结构删除掉，同时表中的数据也删除掉，释放表空间
    DROP TABLE IF EXISTS myemp2;
    DROP TABLE IF EXISTS myemp12;
    ```
6. 清空表
    ```sql
    #清空表，表示清空表中的所有数据，但是表结构保留。
    SELECT * FROM employees_copy;

    TRUNCATE TABLE employees_copy;

    SELECT * FROM employees_copy;

    DESC employees_copy;
    ```
  
  - **DCL 中 COMMIT 和 ROLLBACK**
    - **COMMIT**:提交数据。一旦执行COMMIT，则数据就被永久的保存在了数据库中，意味着数据不可以回滚。
    - **ROLLBACK**:回滚数据。一旦执行ROLLBACK,则可以实现数据的回滚。回滚到最近的一次COMMIT之后。
  - **对比 TRUNCATE TABLE 和 DELETE FROM**
    - 相同点：都可以实现对表中所有数据的删除，同时保留表结构。
    - 不同点：
      - TRUNCATE TABLE：一旦执行此操作，表数据全部清除。同时，数据是不可以回滚的(DDL，不可以回滚)。
      - DELETE FROM：一旦执行此操作，表数据可以全部清除（不带WHERE）。**同时，数据是可以实现回滚的**(DML，默认不可以回滚，使用``SET autocommit = FALSE``可以)。
    > 阿里开发规范：【参考】TRUNCATE TABLE 比 DELETE 速度快，且使用的系统和事务日志资源少，但 TRUNCATE 无事务且不触发 TRIGGER，有可能造成事故，故**不建议**在开发代码中使用此语句。
7. DDL 和 DML 的说明
- **DDL的操作**一旦执行，就不可回滚。指令``SET autocommit = FALSE``对DDL操作失效。(COMMIT操作不受SET autocommit = FALSE影响的。)
- **DML的操作**默认情况，一旦执行，也是不可回滚的。但是，如果在执行DML之前，执行了 ``SET autocommit = FALSE``，则执行的DML操作就可以实现回滚。
  ```sql
  # 演示：DELETE FROM 
  #1)
  COMMIT;
  #2)
  SELECT *
  FROM myemp3;
  #3)
  SET autocommit = FALSE;
  #4)
  DELETE FROM myemp3;
  #5)
  SELECT *
  FROM myemp3;
  #6)
  ROLLBACK; # 可以实现回滚
  #7)
  SELECT *
  FROM myemp3;
  ```

### DML之增删改
1. 插入数据
- VALUES的方式添加
  ```sql
  # ① 没有指明添加的字段
  #正确的
  INSERT INTO emp1
  VALUES (1,'Tom','2000-12-21',3400); #注意：一定要按照声明的字段的先后顺序添加
  #错误的
  INSERT INTO emp1
  VALUES (2,3400,'2000-12-21','Jerry');

  # ② 指明要添加的字段 （推荐）
  INSERT INTO emp1(id,hire_date,salary,`name`)
  VALUES(2,'1999-09-09',4000,'Jerry');
  # 说明：没有进行赋值的hire_date 的值为 null
  INSERT INTO emp1(id,salary,`name`)
  VALUES(3,4500,'shk');

  # ③ 同时插入多条记录 （推荐）
  INSERT INTO emp1(id,NAME,salary)
  VALUES
  (4,'Jim',5000),
  (5,'张俊杰',5500);
  ```
- 将SELECT查询方式
将SELECT语句查询的结果插入到表中，此时不需要把每一条记录的值一个一个输入，只需要使用一条INSERT语句和一条SELECT语句组成的组合语句即可快速地从一个或多个表中向一个表中插入多行。
  ```sql
  INSERT INTO emp1(id,NAME,salary,hire_date)
  SELECT employee_id,last_name,salary,hire_date  # 查询的字段一定要与添加到的表的字段一一对应
  FROM employees
  WHERE department_id IN (70,60);

  INSERT INTO emp2
  SELECT *
  FROM employees
  WHERE department_id = 90;
  ```
  > 说明：emp1表中要添加数据的字段的长度不能低于employees表中查询的字段的长度。如果emp1表中要添加数据的字段的长度低于employees表中查询的字段的长度的话，就有添加不成功的风险。

2. 更新数据
    ``UPDATE .... SET .... WHERE ...``
    ```sql
    #可以同时修改一条数据的多个字段
    UPDATE emp1
    SET hire_date = CURDATE(),salary = 6000
    WHERE id = 4;
    ```
3. 删除数据
    ``DELETE FROM .... WHERE....``
    ```sql
    DELETE FROM emp1
    WHERE id = 1;
    ```
    > DML操作默认情况下，执行完以后都会自动**COMMIT**数据。如果希望执行完以后不自动提交数据，则需要使用 ``SET autocommit = FALSE``.
  

### 数据类型

### 约束
为了保证数据的完整性需要约束。约束是对表中字段的限制。可以**CREATE TABLE 时添加约束**， 或者 **ALTER TABLE 时增加约束、删除约束**。
约束的分类：
- 角度1：约束的字段的个数
  - 单列约束 vs 多列约束
- 角度2：约束的作用范围
  - 列级约束：将此约束声明在对应字段的后面
  - 表级约束：在表中所有字段都声明完，在所有字段的后面声明的约束
- 角度3：约束的作用（或功能）
  - not null (非空约束)
  - unique  (唯一性约束)
  - primary key (主键约束)
  - foreign key (外键约束)
  - check (检查约束)
  - default (默认值约束)
  > 注意： MySQL不支持check约束，但可以使用check约束，而没有任何效果。

查看某个表已有的约束：
```sql
#information_schema数据库名（系统库）
#table_constraints表名称（专门存储各个表的约束）
SELECT * FROM information_schema.table_constraints
WHERE table_name = '表名称';
```
#### 非空约束
**NOT NULL**: 限定某个字段/某列的值不允许为空
> 空字符串''不等于NULL，0也不等于NULL

1. 在CREATE TABLE时添加非空约束
    ```sql
    CREATE TABLE test1(
      id INT NOT NULL, # 约束
      last_name VARCHAR(15) NOT NULL, # 约束
      email VARCHAR(25),
      salary DECIMAL(10,2)
    );
    ```
2. 在ALTER TABLE时添加非空约束
    ```sql
    ALTER TABLE test1
    MODIFY email VARCHAR(25) NOT NULL; # 约束
    ```
3. 在ALTER TABLE时删除非空约束
    ```sql
    ALTER TABLE test1
    MODIFY email VARCHAR(25) NULL; # 删除约束
    # OR
    ALTER TABLE test1
    MODIFY email VARCHAR(25); # 删除约束
    ```

#### 唯一性约束
**UNIQUE**: 用来限制某个字段/某列的值不能重复。同一个表可以有多个唯一约束。唯一约束可以是某一个列的值唯一，也可以多个列组合的值唯一。唯一性约束允许列值为空。在创建唯一约束的时候，如果不给唯一约束命名，就默认和列名相同。**MySQL会给唯一约束的列上默认创建一个唯一索引**。

1. 在CREATE TABLE时添加约束
    ```sql
    CREATE TABLE test2(
      id INT UNIQUE, #列级约束
      last_name VARCHAR(15) ,
      email VARCHAR(25),
      salary DECIMAL(10,2),
      #表级约束
      CONSTRAINT uk_test2_email UNIQUE(email) # [CONSTRAINT 约束名] UNIQUE(字段名)
    );
    ```
    > 可以向声明为unique的字段上添加 NULL 值。而且可以多次添加 NULL
2. 在ALTER TABLE时添加约束
    ```sql
    #方式1：
    ALTER TABLE test2
    ADD CONSTRAINT uk_test2_sal UNIQUE(salary);
    #方式2：
    ALTER TABLE test2
    MODIFY last_name VARCHAR(15) UNIQUE;
    ```
3. 复合的唯一性约束
    ```sql
    CREATE TABLE USER(
      id INT,
      `name` VARCHAR(15),
      `password` VARCHAR(25),
      
      #表级约束
      CONSTRAINT uk_user_name_pwd UNIQUE(`name`,`password`)
    ); # 多个字段的组合是唯一的
    ```
4. 删除唯一性约束
   - 添加唯一性约束的列上也会自动创建唯一索引。
   - 删除唯一约束只能通过删除唯一索引的方式删除。
   - 删除时需要指定**唯一索引名**，唯一索引名就和唯一约束名一样。
   - 如果创建唯一约束时未指定名称，如果是单列，就默认和列名相同；如果是组合列，那么默认和()中排在第一个的列名相同。也可以自定义唯一性约束名。
    ```sql
    #如何删除唯一性索引
    ALTER TABLE test2
    DROP INDEX last_name;

    ALTER TABLE test2
    DROP INDEX uk_test2_sal;
    ```
#### 主键约束
**PRIMARY KEY**: 用来唯一标识表中的一行记录。主键约束相当于**唯一约束+非空约束**的组合，主键约束列不允许重复，也不允许出现空值。一个表中最多只能有一个主键约束。主键约束对应着表中的一列或者多列（复合主键）
> MySQL的主键名总是PRIMARY，就算自己命名了主键约束名也没用。
1. 在CREATE TABLE时添加约束
    ```sql
    # 主键约束特征：非空且唯一，用于唯一的标识表中的一条记录。
    CREATE TABLE test4(
    id INT PRIMARY KEY, #列级约束
    last_name VARCHAR(15),
    salary DECIMAL(10,2),
    email VARCHAR(25)
    );

    CREATE TABLE user1(
    id INT,
    NAME VARCHAR(15),
    PASSWORD VARCHAR(25),
    PRIMARY KEY (NAME,PASSWORD)
    );#如果是多列组合的复合主键约束，那么这些列都不允许为空值，并且组合的值不允许重复。
    ```
2. 在ALTER TABLE时添加约束
    ```sql
    ALTER TABLE 表名称 
    ADD PRIMARY KEY(字段列表); 
    #字段列表可以是一个字段，也可以是多个字段，如果是多个字段的话，是复合主键
    ```
3. 删除主键约束 
    在实际开发中，不会去删除表中的主键约束！
    ```sql
    ALTER TABLE 表名称 
    DROP PRIMARY KEY;
    ```

#### 自增列
**AUTO_INCREMENT**: 某个字段的值自增。
特点：
- 一个表最多只能有一个自增长列
- 当需要产生唯一标识符或顺序值时，可设置自增长
- 自增长列约束的列必须是键列（主键列，唯一键列）
- 自增约束的列的数据类型必须是整数类型
- **如果自增列指定了 0 和 null，会在当前最大值的基础上自增；如果自增列手动指定了具体值，直接赋值为具体值**。
1. 在CREATE TABLE时添加约束
    ```sql
    CREATE TABLE test7(
    id INT PRIMARY KEY AUTO_INCREMENT,
    last_name VARCHAR(15) 
    );
    ```
2. 在ALTER TABLE时添加约束
    ```sql
    CREATE TABLE test8(
    id INT PRIMARY KEY ,
    last_name VARCHAR(15) 
    );

    ALTER TABLE test8
    MODIFY id INT AUTO_INCREMENT;
    ```
3. 在ALTER TABLE 时删除
    ```sql
    ALTER TABLE test8
    MODIFY id INT;
    ```
#### 外键约束
**FOREIGN KEY**: 限定某个表的某个字段的引用完整性。
举个例子：员工表的员工所在部门的选择，必须在部门表能找到对应的部分。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/FOREIGN_KEY.png"/>
主表（父表）：被引用的表，被参考的表
从表（子表）：引用别人的表，参考别人的表
例如：员工表的员工所在部门这个字段的值要参考部门表：部门表是主表，员工表是从表。
**从表的外键列引用主表的被参照列**，从表的外键列与主表被参照的列名字可以不相同，但是数据类型必须一样。**从表的外键列，必须引用/参考主表的主键或唯一约束的列**。在创建外键约束时，如果不给外键约束命名，**默认名不是列名，而是自动产生一个外键名**。
**创建表时，先创建主表，再创建从表；删表时，先删从表（或先删除外键约束），再删除主表**。

1. 在CREATE TABLE时添加约束
    ```sql
    #①先创建主表
    CREATE TABLE dept1(
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(15)
    );
    #②再创建从表
    CREATE TABLE emp1(
    emp_id INT PRIMARY KEY AUTO_INCREMENT,
    emp_name VARCHAR(15),
    department_id INT,
    #表级约束
    CONSTRAINT fk_emp1_dept_id FOREIGN KEY (department_id) REFERENCES dept1(dept_id)
    );
    ```
2. 在ALTER TABLE时添加外键约束
    ```sql
    CREATE TABLE dept2(
    dept_id INT PRIMARY KEY,
    dept_name VARCHAR(15)
    );

    CREATE TABLE emp2(
    emp_id INT PRIMARY KEY AUTO_INCREMENT,
    emp_name VARCHAR(15),
    department_id INT
    );

    ALTER TABLE emp2
    ADD CONSTRAINT fk_emp2_dept_id FOREIGN KEY(department_id) REFERENCES dept2(dept_id);

    SELECT * FROM information_schema.table_constraints 
    WHERE table_name = 'emp2';
    ```
3. 约束等级
   - Cascade方式：在父表上update/delete记录时，同步update/delete掉子表的匹配记录
   - Set null方式：在父表上update/delete记录时，将子表上匹配记录的列设为null，但是要注意子表的外键列不能为not null
   - No action方式：如果子表中有匹配的记录，则不允许对父表对应候选键进行update/delete操作
   - Restrict方式：同no action， 都是立即检查外键约束
   - Set default方式（在可视化工具SQLyog中可能显示空白）：父表有变更时，子表将外键列设置成一个默认的值，但Innodb不能识别
  
    对于外键约束，最好是采用: ``ON UPDATE CASCADE ON DELETE RESTRICT`` 的方式。
    ```sql
    #演示：
    # on update cascade on delete set null
    CREATE TABLE dept(
        did INT PRIMARY KEY,		#部门编号
        dname VARCHAR(50)			#部门名称
    );

    CREATE TABLE emp(
        eid INT PRIMARY KEY,  #员工编号
        ename VARCHAR(5),     #员工姓名
        deptid INT,		  #员工所在的部门
        FOREIGN KEY (deptid) REFERENCES dept(did)  ON UPDATE CASCADE ON DELETE SET NULL
        #把修改操作设置为级联修改等级，把删除操作设置为set null等级
    );

    INSERT INTO dept VALUES(1001,'教学部');
    INSERT INTO dept VALUES(1002, '财务部');
    INSERT INTO dept VALUES(1003, '咨询部');

    INSERT INTO emp VALUES(1,'张三',1001); #在添加这条记录时，要求部门表有1001部门
    INSERT INTO emp VALUES(2,'李四',1001);
    INSERT INTO emp VALUES(3,'王五',1002);

    UPDATE dept
    SET did = 1004
    WHERE did = 1002;

    DELETE FROM dept
    WHERE did = 1004;
    ```
4. 删除外键约束
    ```sql
    (1)第一步先查看约束名和删除外键约束
    SELECT * FROM information_schema.table_constraints WHERE table_name = '表名称';#查看某个表的约束名
    ALTER TABLE 从表名 DROP FOREIGN KEY 外键约束名;
    （2）第二步查看索引名和删除索引。（注意，只能手动删除）
    SHOW INDEX FROM 表名称; #查看某个表的索引名
    ALTER TABLE 从表名 DROP INDEX 索引名;
    ```
> 外键虽然能够保证字段引用完整性，但建外键约束，使得操作（创建表、删除表、添加、修改、删除）会受到限制。在 MySQL 里，外键约束是有成本的，需要消耗系统资源。所以， MySQL 允许你不使用系统自带的外键约束，在应用层面完成检查数据一致性的逻辑。阿里开发规范：【强制】**不得使用外键与级联，一切外键概念必须在应用层解决**。
#### 检查约束
**CHECK**： 检查某个字段的值是否符号xx要求，一般指的是值的范围
```sql
CREATE TABLE test10(
id INT,
last_name VARCHAR(15),
salary DECIMAL(10,2) CHECK(salary > 2000)
);
```
#### 默认值约束
**DEFAULT**: 给某个字段/某列指定默认值，一旦设置默认值，在插入数据时，如果此字段没有显式赋值，则赋值为默认值。
1. 在CREATE TABLE时添加约束
    ```sql
    CREATE TABLE test11(
    id INT,
    last_name VARCHAR(15),
    salary DECIMAL(10,2) DEFAULT 2000
    #字段名 数据类型 NOT NULL DEFAULT 默认值,
    #字段名 数据类型 DEFAULT 默认值,
    );
    ```
2. 在ALTER TABLE时添加约束
    ```sql
    CREATE TABLE test12(
    id INT,
    last_name VARCHAR(15),
    salary DECIMAL(10,2)
    );

    DESC test12;
    ALTER TABLE test12
    MODIFY salary DECIMAL(8,2) DEFAULT 2500;
    ```
3. 在ALTER TABLE 时删除
    ```sql
    ALTER TABLE test12
    MODIFY salary DECIMAL(8,2);
    ```

## 逻辑架构与存储引擎






## 索引及调优
### 索引的数据结构
#### 索引及其优缺点
MySQL官方对索引的定义为：**索引（Index）是帮助MySQL高效获取数据的数据结构**。目的就是为了**减少磁盘I/O的次数，加快查询速率**。
**索引是在存储引擎中实现的**，因此每种存储引擎的索引不一定完全相同，并且每种存储引擎不一定支持所有索引类型。同时，存储引擎可以定义每个表的最大索引数和最大索引长度。所有存储引擎支持每个表至少16个索引，总索引长度至少为256字节。有些存储引擎支持更多的索引数和更大的索引长度。

**优点**：
- 类似大学图书馆建书目索引，提高数据检索的效率，降低**数据库的IO成本**，这也是创建索引最主要的原因。
- 通过创建唯一索引，可以保证数据库表中每一行 数据的唯一性。
- 在实现数据的参考完整性方面，可以**加速表和表之间的连接**。换句话说，对于有依赖关系的子表和父表联合查询时，可以提高查询速度。
- 在使用分组和排序子句进行数据查询时，可以显著**减少查询中分组和排序的时间**，降低了CPU的消耗。

**缺点**：
增加索引也有许多不利的方面，主要表现在如下几个方面：

- 创建索引和维护索引要**耗费时间**，并且随着数据量的增加，所耗费的时间也会增加。
- 索引需要占**磁盘空间**，除了数据表占数据空间之外，每一个索引还要占一定的物理空间， 存储在磁盘上 ，如果有大量的索引，索引文件就可能比数据文件更快达到最大文件尺寸。
- 虽然索引大大提高了查询速度，同时却会**降低更新表的速度**。当对表中的数据进行增加、删除和修改的时候，索引也要动态地维护，这样就降低了数据的维护速度。
  
因此，选择使用索引时，需要综合考虑索引的优点和缺点。

#### InnoDB中索引

InnoDB中的索引是一个B+树，**页之间是双向链表，页内记录是单向链表**。通过索引快速访问到对应的页。页内记录可以在**页目录**中使用**二分法**快速定位到对应的槽，然后再遍历该槽对应分组中的记录即可快速找到指定的记录。
![](images/index_B+_tree.png)
- record_type ：记录头信息的一项属性，表示记录的类型， 0 表示普通记录、 2 表示最小记录、 3 表示最大记录、 1 表示目录项记录
- next_record ：记录头信息的一项属性，表示下一条地址相对于本条记录的地址偏移量，我们用箭头来表明下一条记录是谁。
- 各个列的值 ：这里只记录在 index_demo 表中的三个列，分别是 c1 、 c2 和 c3 。

如图所示：
- B+树的叶子节点是**用户记录页**（只有叶子节点才存数据），每个页存放3条用户记录。
- B+树的**非叶子节点**（内点）是**目录项记录页**（每个页存放4条数据），目录项记录只有**主键值和页的编号**。最上面的节点是**根节点**。

> **B+树的层越低I/O次数越好，4层能够存储足够多的数据**。实际中，用户页可以存100条用户记录，目录项记录页可以存放1000条目录项记录。4层B+树，最多能存放：1000×1000×1000×100=1000,0000,0000 条记录。



<h5 align="left"> 常见索引概念 </h5>

索引按照物理实现方式，索引可以分为 2 种：**聚簇**（聚集）和**非聚簇**（非聚集）索引。我们也把非聚集索引称为二级索引或者辅助索引。
1. 聚簇索引
  
**聚簇索引**并不是一种单独的索引类型，而是一种**数据存储方式**(所有的用户记录都存储在了叶子节点)，也就是所谓的**索引即数据，数据即索引**。

**特点：**
- 使用记录主键值的大小进行记录和页的排序，这包括三个方面的含义：
  - **页内**的记录是按照主键的大小顺序排成一个**单向链表**。
  - 各个**存放用户记录的页**也是根据页中用户记录的主键大小顺序排成一个**双向链表**。
  - 存放目录项记录的页分为不同的层次，在同一层次中的页也是根据页中目录项记录的主键大小顺序排成一个**双向链表**。
- B+树的叶子节点存储的是完整的用户记录：所谓完整的用户记录，就是指这个记录中存储了所有列的值（包括隐藏列）。
  
**优点：**
- **数据访问更快**，因为聚簇索引将索引和数据保存在同一个B+树中，因此从聚簇索引中获取数据比非聚簇索引更快
- 聚簇索引对于主键的**排序查找**和**范围查找**速度非常快
- 按照聚簇索引排列顺序，查询显示一定范围数据的时候，由于数据都是紧密相连，数据库不用从多
个数据块中提取数据，所以**节省了大量的io操作**。(**没有主键Innodb会选择唯一的非空索引，找不到Innodb隐式定义**)

**缺点：**
- **插入速度严重依赖于插入顺序**，按照主键的顺序插入是最快的方式，否则将会出现页分裂，严重影响性能。因此，对于InnoDB表，我们一般都会定义一个**自增的ID列为主键**
- **更新主键的代价很高**，因为将会导致被更新的行移动。因此，对于InnoDB表，我们一般定义**主键为不可更新**
- **二级索引访问需要两次索引查找**，第一次找到主键值，第二次根据主键值找到行数据

2. 二级索引（辅助索引、非聚簇索引）
   
前面以主键为搜索条件，如果以其他列为搜索条件呢？这里以c2列为例。用c2列的大小作为数据页、页中记录的排序规则，再建一棵B+树。

这个B+树与上边介绍的聚簇索引有几处不同:
- 使用记录c2列的大小进行记录和页的排序，这包括三个方面的含义:
  - 页内的记录是按照c2列的大小顺序排成一个单向链表。
  - 各个存放用户记录的页也是根据页中记录的c2列大小顺序排成一个双向链表。
  - 存放目录项记录的页分为不同的层次，在同一层次中的页也是根据页中目录项记录的c2列大小顺序排成个双向链表。
- B+树的叶子节点存储的并不是完整的用户记录，而只是c2列+主键这两个列的值。
- 目录项记录中不再是主键+页号的搭配，而变成了c2列+页号的搭配。

**回表：** 我们根据这个以c2列大小排序的B+树只能确定我们要查找记录的主键值，然后**根据找到的主键到聚簇索引中再查一遍**来找到完整数据记录，这个过程称为回表。（也就是根据c2列的值查询一条完整的用户记录需要使用到 2 棵B+树！）（如果不回表太浪费存储空间）

**小结**:聚簇索引与非聚簇索引的原理不同，在使用上也有一些区别:
- 聚簇索引的叶子节点存储的就是我们的数据记录，非聚簇索引的叶子节点存储的是数据位置。非聚簇索引不会影响数据表的物理存储顺序。
- **一个表只能有一个聚簇索引**，因为只能有一种排序存储的方式，但可以有多个非聚簇索引，也就是多个索引目录提供数据检索。
- 使用聚簇索引的时候，数据的查询效率高，但如果对数据进行插入，删除，更新等操作，效率会比非聚簇索引低。

3. 联合索引

联合索引其实就是非聚簇索引，同时以多个列的大小作为排序规则。想让B+树按照c2和c3列的大小进行排序，这个包含两层含义：
- 先把各个记录和页按照c2列进行排序。
- 在记录的c2列相同的情况下，采用c3列进行排序

<h5 align="left"> InnoDB的B+树索引的注意事项 </h5>

1. **根页面位置万年不动**
- 每当为某个表创建一个B+树索引(聚簇索引不是人为创建的，默认就有）的时候，都会为这个索引创建一个根节点页面，其中既没有用户记录，也没有目录项记录。
- 随后向表中插入用户记录时，先把用户记录存储到这个根节点中。
- 当根节点中的可用空间用完时继续插入记录，此时会将根节点中的所有记录复制到一个新分配的页，比如页a中，然后对这个新页进行页分裂的操作，得到另一个新页，比如页b。这时新插入的记录会根据键值（也就是聚簇索引中的主键值，二级索引中对应的索引列的值)的大小就会被分配到页a或者页b中，而根节点便升级为存储目录项记录的页。
2. **内节点中目录项记录的唯一性**
**保证在B+树的同一层内节点的目录项记录除页号这个字段以外是唯一的**。所以对于二级索引的内节点的目录项记录的内容实际上是由三个部分构成的:索引列的值、主键值、页号（添加了主键）
3. **一个页面最少存储2条记录**

#### MyISAM中的索引

MyISAM引擎使用B+Tree 作为索引结构，将索引和数据分开存储，叶子节点的data域存放的是**数据记录的地址**。
![](images/index_myisam.png)

**MyISAM 与 InnoDB对比**
- 在InnoDB存储引擎中，我们只需要根据主键值对**聚簇索引**进行一次查找就能找到对应的记录，而在MyISAM 中却需要进行一次**回表**操作，意味着MyISAM中建立的索引相当于全部都是**非聚簇索引**。
- InnoDB的数据文件本身就是索引文件，而MyISAM索引文件和数据文件是 分离的 ，索引文件仅保存数据记录的地址。
- InnoDB的非聚簇索引data域存储相应记录**主键的值**，而MyISAM索引记录的是**地址**。换句话说，InnoDB的所有非聚簇索引都引用主键作为data域。
- MyISAM的回表操作是十分**快速**的，因为是拿着地址偏移量直接到文件中取数据的，反观InnoDB是通过获取主键之后再去聚簇索引里找记录，虽然说也不慢，但还是比不上直接用地址去访问
- InnoDB要求表**必须有主键**（ MyISAM可以没有 ）。
  
#### 索引的代价

- 空间上的代价：每建立一个索引都要为它建立一棵B+树，每一棵**B+树的每一个节点都是一个数据页**，一个页默认会占用 16KB 的存储空间，一棵很大的B+树由许多数据页组成，那就是很大的一片存储空间。
- 时间上的代价：每次对表中的数据进行**增、删、改**操作时，都需要去修改各个B+树索引。而增、删、改操作可能会对节点和记录的排序造成破坏，所以存储引擎需要额外的时间进行一些 **记录移位、页面分裂、页面回收**等操作来维护好节点和记录的排序。

#### MySQL数据结构选择的合理性

全表遍历, Hash结构, 二叉搜索树, AVL树(略)

<h5 align="left"> B-Tree </h5>
B-Tree(多路平衡查找树)

![](images/index_B_tree.png)
内点页存放数据。

**B+ 树和 B 树的差异**：
- B+树有 k 个孩子的节点就有 k 个关键字。也就是孩子数量 = 关键字数，而 B 树中，孩子数量 = 关键字数+1。
- B+树非叶子节点仅用于索引，不保存数据记录。而 B 树中， 非叶子节点既保存索引，也保存数据记录。
> B+树索引段和数据段的区分有利于顺序 I/O



**思考题：为了减少IO，索引树会一次性加载吗？**
1. 数据库索引是存储在磁盘上的，如果数据量很大，必然导致索引的大小也会很大，超过几个G。
2. 当我们利用索引查询时候，是不可能将全部几个G的索引都加载进内存的，我们能做的只能是:逐一加载每一个磁盘页，因为磁盘页对应着索引树的节点。

**思考题：B+树的存储能力如何？为何说一般查找行记录，最多只需1~3次磁盘IO？**
InnoDB存储弓|擎中页的大小为16KB, 一般表的主键类型为INT (占用4个字节)或BIGINT (占用8个字节)，指针类型也一般为4或8个字节，也就是说一个页(B+Tree 中的一个节点)中大概存储16KB/(8B+8B)=1K个键值(因为是估值，为方便计算，这里的K取值为$10^3$。 也就是说一个深度为3的B+Tree索引可以维护$10^3 * 10^3 * 10^3 = 10$亿条记录。(这里假定一个数据页也存储10^3条行记录数据了)实际情况中每个节点可能不能填充满，因此在数据库中，**B+Tree 的高度一般都在2~4层**。MySQL 的InnoDB存储引擎在设计时是将**根节点常驻内存**的，也就是说查找某一键值的行记录时**最多只需要 1~3次磁盘I/O操作**。

**思考题：为什么说B+树比B-树更适合实际应用中操作系统的文件索引和数据库索引？**
1. B+树的磁盘读写代价更低
B+树的内部结点并没有指向关键字具体信息的指针。因此其内部结点相对B树更小。如果把所有同一内部结点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多。一次性读入内存中的需要查找的关键字也就越多。相对来说I0读写次数也就降低了。
2. B+树的查询效率更加稳定
由于非终结点并不是最终指向文件内容的结点，而只是叶子结点中关键字的索引。所以任何关键字的查找必须走一条从根结点到叶子结点的路。 所有关键字查询的路径长度相同，导致每一个数据的查询效率相当。

**思考题：Hash 索引与 B+ 树索引的区别**
1. **Hash索引不能进行范围查询**，而B+树可以。这是因为Hash索引指向的数据是无序的，而B+树的叶子节点是个有序的链表。
2. **Hash 索引不支持联合索引的最左侧前缀原则**(即联合索引的部分索引无法使用)，而B+树可以。对于联合索引来说，Hash 索引在计算Hash值的时候是将索引键合并后再一起计算Hash值，所以不会针对每个索引单独计算Hash值。因此如果用到联合索引的一个或者几个索引时，联合索引无法被利用。
3. **Hash 索引不支持ORDER BY排序**，因为Hash索引指向的数据是无序的，因此无法起到排序优化的作用，而B+树索引数据是有序的，可以起到对该字段ORDER BY排序优化的作用。同理，我们也无法用Hash索引进行模糊查询，而B+树使用LIKE进行模糊查询的时候，LIKE 后面后模糊查询(比如%结尾)的话就可以起到优化作用
4. **innodb不支持哈希索引**

**思考题：Hash 索引与 B+ 树索引是在建索引的时候手动指定的吗？**
![](images/index_type_for_storage_engine.png)
针对InnoDB和MyISAM存储引擎，都会默认采用B+树索引，无法使用Hash索引。InnoDB 提供的自适应Hash是不需要手动指定的。如果是Memory/Heap和NDB存储引擎，是可以进行选择Hash索引的。


### InnoDB数据存储结构
#### 页及页的上层结构
InnoDB页默认**16KB**，以页作为磁盘和内存之间交互的**基本单位**。**数据库管理存储的空间的基本单位是页（PAGE），数据库I/O操作的最小单位是页**。页与页之间在物理结构上不连续，通过**双向链表**实现逻辑连续。页内的记录通过**单向链表**连接，每个数据页都会为存储在里边的记录生成**页目录**（分组），然后二分查找（后面详细论述）。

在数据库中页的上层结构还包括区(Extent), 段(Segment)和表空间(Tablespace)。行、页、区、段、表空间的关系如下图：
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/inpage_upper_structure.png"/>

**区 (Extent)** 是比页大一级的存储结构，在 InnoDB 存储引擎中，一个**区会分配 64 个连续的页**。因为 InnoDB 中 的页大小默认是16 KB，所以一个区的大小是**64*16KB=1MB**。在表**数据量大**，为某个索引分配空间不在按照页为单位，而是以**区为单位**进行分配(顺序I/O保证查询的高效性)。

**段 (Segment)** 由一个或多个区组成，区与区之间可以是不相邻的。InnoDB对B+树的叶子节点和非叶子节点分开存储，存放叶子节点区的集合是**数据段**， 存放非叶子节点的段叫**索引段**，此外InnoDB还有存储一些特殊的数据而定义的段，如**回滚段**。

默认情况下InnoDB一个聚簇索引会生成两个段，段以区为单位，所以**默认情况下只存几条记录的小表也需要2M的存储空间**，造成存储空间的浪费。为了解决**数据量较小**造成存储空间浪费情况，InnoDB提出**碎片(fragment)区**概念，**碎片区直属于表空间**，并不属于任何一个段（碎片区中的不同页可以用于不同的段）。所以此后为某个段分配存储空间的策略是这样的:
- 在刚开始向表中插入数据的时候，段是从某个碎片区以单个页面为单位来分配存储空间的。
- 当某个段已经占用了 **32个碎片区**页面之后，就会申请以完整的区为单位来分配存储空间。

**区的分类**
区大体上可以分为4种类型:
- **空闲的区(FREE)**：现在还没有用到这个区中的任何页面。|
- **有剩余空间的碎片区(FREE_FRAG)**：表示碎片区中还有可用的页面。
- **没有剩余空间的碎片区(FULL_FRAG)**：表示碎片区中的所有页面都被使用，没有空闲页面。
- **附属于某个段的区 (FSEG)**：每一个索引都可以分为叶子节点段和非叶子节点段。

处于**FREE 、FREE_FRAG 以及 FULL_FRAG**这三种状态的区都是独立的，**直属于表空间**。而处于FSEG 状态的区是附属于某个段的。


**表空间**（Tablespace）是一个**逻辑容器**，表空间存储的对象是段，在一个表空间中可以有一个或多个段，但是一个段只能属于一个表空间。数据库由一个或多个表空间组成，表空间从管理上可以划分为**系统表空间**（System tablespace）、**独立表空间** (File-per-table tablespace）、**撤销表空间** (Undo Tablespace) 和**临时表空间** (Temporary Tablespace) 等。
- **独立表空间**: 每张表有一个独立的表空间，独立表空间可以在不同数据库**迁移**。独立表空间由段、区、页组成。查看InnoDB的表空间类型：``mysql > show variables like 'innodb_file_per_table';``，``innodb_file_per_table=ON``每张表都会单独保存为一个.ibd 文件。
- **系统表空间**: 和独立表空间类似，整个MySQL进程只有一个系统表空间，系统表空间会额外记录一些有关整个系统信息的页面。


#### 页的内部结构
页按类型分为``数据页(保存B+树节点)``、``系统页``、``Undo页``和``事物数据页``等。数据页(16KB)最常见，被划分七个部分，分别是文件头 (File Header) 、页头 (Page Header) 、最大 最小记录（Infimum+supremum）、用户记录 (User Records) 、空闲空间 (Free Space) 、页目录 (Page Directory) 和文件尾 (File Tailer)。
|名称    | 占用大小   |  说明    | 
|:----    | :---:    | :--- |
|File Header | 38字节 |文件头，描述页的信息  |
|Page Header |  56字节|页头，页的状态信息 |
|Infimum+Supremum |26字节| 最大和最小记录，这是两个虚拟的行记录 |
|User Records |不确定| 用户记录，存储行记录内容 |
|Free Space |不确定| 空闲记录，页中还没有被使用的空间 |
|Page Directory |不确定| 页目录，存储用户记录的相对位置 |
|File Trailer |8字节| 文件尾，校验页是否完整 |
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_page_structure.png"/>

**第1部分**
1. File Header（文件头部）（38字节）
描述各种页的通用信息。（比如页的编号、其上一页、下一页是谁等）
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_page_file_header.png"/>

- FIL_PAGE_OFFSET（4字节）: 每一个页都有一个单独的页号，就跟你的身份证号码一样，InnoDB通过页号可以**唯一**定位一个页。
- FIL_PAGE_TYPE（2字节）: 这个代表当前页的类型。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_page_file_header_type.png"/>

- FIL_PAGE_PREV（4字节）和FIL_PAGE_NEXT（4字节）: InnoDB都是以页为单位存放数据的，如果数据分散到多个不连续的页中存储的话需要把这些页关联起来，FIL_PAGE_PREV和FIL_PAGE_NEXT就分别代表本页的上一个和下一个页的页号。这样通过建立一个双向链表把许许多多的页就都串联起来了，保证这些页之间不需要是物理上的连续，而是逻辑上的连续。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_page_file_header_prev_next.png"/>

- FIL_PAGE_SPACE_OR_CHKSUM（4字节）: 代表当前页面的校验和（checksum）。文件头部和文件尾部都有属性：FIL_PAGE_SPACE_OR_CHKSUM。
  - **校验和**：就是对于一个很长的字节串来说，我们会通过某种算法来计算一个比较短的值来代表这个很长的字节串，这个比较短的值就称为校验和。在比较两个很长的字节串之前，先比较这两个长字节串的校验和，如果校验和都不一样，则两个长字节串肯定是不同的，所以省去了直接比较两个比较长的字节串的时间损耗。
  - **作用**：InnoDB存储引擎以页为单位把数据加载到内存中处理，如果该页中的数据在内存中被修改了，那么在修改后的某个时间需要把数据同步到磁盘中。但是在同步了一半的时候断电了，造成了该页传输的不完整。
  为了检测一个页是否完整（也就是在同步的时候有没有发生只同步一半的尴尬情况），这时可以通过文件尾的校验和（checksum 值）与文件头的校验和做比对，如果两个值不相等则证明页的传输有问题，需要重新进行传输，否则认为页的传输已经完成。校验方式就是采用 Hash 算法进行校验。

- FIL_PAGE_LSN（8字节）：页面被最后修改时对应的日志序列位置（英文名是：Log Sequence Number）
2. File Trailer（文件尾部）（8字节）
- 前4个字节代表页的校验和：这个部分是和File Header中的校验和相对应的。
- 后4个字节代表页面被最后修改时对应的日志序列位置（LSN）：这个部分也是为了校验页的完整性的，如果首部和尾部的LSN值校验不成功的话，就说明同步过程出现了问题。

**第2部分**

记录部分，页的主要作用是存储记录，所以“最大和最小记录”和“用户记录”部分占了页结构的主要空间。

3. Free Space (空闲空间)
我们自己存储的记录会按照指定的**行格式**存储到**User Records**部分。但是在一开始生成页的时候，其实并没有User Records这个部分，**每当我们插入一条记录，都会从Free Space部分，也就是尚未使用的存储空间中申请一个记录大小的空间划分到User Records部分**，当Free Space部分的空间全部被User Records部分替代掉之后，也就意味着这个页使用完了，如果还有新的记录插入的话，就需要去**申请新的页**了。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_page_free_space.png"/>

4. User Records (用户记录)：User Records中的这些记录按照指定的行格式一条一条摆在User Records部分，相互之间形成单链表。用户记录里的一条条数据如何记录？
这里需要讲讲记录行格式的**记录头信息**（下文）。

5. Infimum + Supremum（最小最大记录）（26字节）:
InnoDB规定的最小记录与最大记录这两条记录的构造十分简单，都是由**5字节大小的记录头信息和8字节大小的一个固定的部分**组成的，如图所示：
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_page_infimum_supremum.png"/>
这两条记录**不是我们自己定义的记录**，所以它们并不存放在页的User Records部分，他们被单独放在一个称为Infimum + Supremum的部分，如图所示：
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_page_infimum_supremum2.png"/>

**第3部分**

6. Page Directory（页目录）
为了方便在**页内**方便查找，**专门给记录做一个目录**，通过**二分查找法**的方式进行检索，提升效率。
- 将所有的记录分成几个组，这些记录包括最小记录和最大记录，但不包括标记为“已删除”的记录。
- **第 1 组，也就是最小记录所在的分组只有 1 个记录；最后一组，就是最大记录所在的分组，会有 1-8 条记录**； 其余的组记录数量在 4-8 条之间。这样做的好处是，除了第 1 组（最小记录所在组）以外，其余组的记录数会**尽量平分**。
- 在每个组中最后一条记录的头信息中会存储该组一共有多少条记录，作为 n_owned 字段。
- **页目录用来存储每组最后一条记录的地址偏移量**，这些地址偏移量会按照**先后顺序**存储起来，每组的地址偏移量也被称之为**槽（slot）**，每个槽相当于指针指向了不同组的最后一个记录。
**例：** 页里一共有6条记录了（包括最小和最大记录），这些记录被分成了2个组，如图所示：
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_page_directory.png"/>


7. Page Header（页面头部）（56字节）
为了能得到一个数据页中存储的记录的状态信息，比如本页中已经存储了多少条记录，第一条记录的地址是什么，页目录中存储了多少个槽等等，特意在页中定义了一个叫Page Header的部分，这个部分占用固定的56个字节，专门存储各种状态信息。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_page_header.png"/>


**B+ 树是如何进行记录检索的?**
如果通过 B+ 树的索引查询行记录，首先是从 B+ 树的根开始，逐层检索，直到找到叶子节点，也就是找到对应的 数据页为止，将数据页加载到内存中，页目录中的槽 (slot) 采用二分查找的方式先找到一个粗略的记录分组， 然后再在分组中通过 链表遍历的方式查找记录。

**普通索引和唯一索引在查询效率上有什么不同?**
我们创建索引的时候可以是普通索引，也可以是唯一索引，那么这两个索引在查询效率上有什么不同呢?
唯一索引就是在普通索引上增加了约束性，也就是关键字唯一，找到了关键字就停止检索。而普通索引，可能会 存在用户记录中的关键字相同的情况，根据页结构的原理，当我们读取一条记录的时候，不是单独将这条记录从 磁盘中读出去，而是将这个记录所在的页加载到内存中进行读取。InnoDB 存储引㢣的页大小为  16 KB ，在一个页 中可能存储着上千个记录，因此在普通索引的字段上进行查找也就是在内存中多几次“判断下一条记录”的操作， 对于 CPU 来说，这些操作所消耗的时间是可以忽略不计的。所以对一个索引字段进行检索，采用普通索引还是唯 一索引在检索效率上基本上没有差别。

#### InnoDB行格式
我们平时的数据以行为单位来向表中插入数据，这些记录在磁盘上的存放方式也被称为`行格式`或者`记录格式`。InnoDB存储引擎设计了4种不同类型的`行格式`，分别是`Compact`、`Redundant`、`Dynamic`和`Compressed`行格式。MySQL(5.7, 8.0)的默认行格式是Dynamic， Dynamic和Compact差别不大。

在创建或修改表的语句中指定行格式：
```
CREATE TABLE 表名 (列的信息) ROW_FORMAT=行格式名称
or
ALTER TABLE 表名 ROW_FORMAT=行格式名称
```
举例：
```shell
mysql> CREATE TABLE record_test_table (
    ->     col1 VARCHAR(8),
    ->     col2 VARCHAR(8) NOT NULL,
    ->     col3 CHAR(8),
    ->     col4 VARCHAR(8)
    -> ) CHARSET=ascii ROW_FORMAT=COMPACT;
Query OK, 0 rows affected (0.03 sec)
```

**COMPACT行格式**

在MySQL 5.1版本中，默认设置为Compact行格式。一条完整的记录其实可以被分为记录的额外信息和记录的真实数据两大部分。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_compact.png"/>

1. 变长字段长度列表
**在Compact行格式中，把所有变长字段的真实数据占用的字节长度都存放在记录的开头部位，从而形成一个变长字段长度列表。**
> 注意：这里面存储的变长长度和字段**顺序是反过来**的

2. NULL值列表
Compact行格式会把可以为NULL的列统一管理起来，存在一个标记为NULL值列表中。如果表中没有允许存储 NULL 的列，则 NULL值列表也不存在了。(自动跳过主键)
- 二进制位的值为1时，代表该列的值为NULL。
- 二进制位的值为0时，代表该列的值不为NULL。
**例**：字段 a、b、c，其中a是主键，在某一行中存储的数依次是 a=1、b=null、c=2。那么Compact行格式中的NULL值列表中存储：01。第一个0表示c不为null，第二个1表示b是null（**顺序也反过来**）。

3. 记录头信息（5字节）：
```shell
mysql> CREATE TABLE page_demo(
    ->     c1 INT,
    ->     c2 INT,
    ->     c3 VARCHAR(10000),
    ->     PRIMARY KEY (c1)
    -> ) CHARSET=ascii ROW_FORMAT=Compact;
Query OK, 0 rows affected (0.03 sec)
```
这个表中记录的行格式示意图：
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_compact_record_header.png"/>

- delete_mask(1 bit): 属性标记着当前记录是否被删除，占用1个二进制位。(0:没被删除， 1被删除)
  > 被删除的记录还在页中存储在真实磁盘上: 这些被删除的记录之所以不立即从磁盘上移除，是因为移除它们之后其他的记录在磁盘上需要**重新排列，导致性能消耗**。所以只是打一个删除标记而已，所有被删除掉的记录都会组成一个所谓的**垃圾链表**，在这个链表中的记录占用的空间称之为**可重用空间**，之后如果有新记录插入到表中的话，可能把这些被删除的记录占用的存储空间**覆盖**掉。
- min_rec_mask(1 bit): B+树的每层非叶子节点中的最小记录都会添加该标记，min_rec_mask值为1。
- n_owned(4 bit): 页目录中每个组中最后一条记录的头信息中会存储该组一共有多少条记录，作为 n_owned 字段。
- heap_no(13 bit): 这个属性表示当前记录在本页中的位置。MySQL会自动给每个页里加了两个**伪记录**（最小，最大记录），**最小记录和最大记录的heap_no值分别是0和1**
- record_type(3 bit): 这个属性表示当前记录的类型，一共有4种类型的记录：
  - 0：表示普通记录
  - 1：表示B+树非叶节点记录
  - 2：表示最小记录
  - 3：表示最大记录 
- next_record(16 bit):
  - 删除操作：删掉第2条记录后的示意图就是
  <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_compact_record_header_next_record.png"/>
    - 第2条记录并没有从存储空间中移除，而是把该条记录的delete_mask值设置为1。
    - 第2条记录的next_record值变为了0，意味着该记录没有下一条记录了。
    - 第1条记录的next_record指向了第3条记录。
    - 最大记录的n_owned值从 5 变成了 4 。 

  - 添加操作：把刚删掉第2条记录重新添加回去
  <img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_compact_record_header_next_record2.png"/> 
  > **不论我们怎么对页中的记录做增删改操作，InnoDB始终会维护一条记录的单链表，链表中的各个节点是按照主键值由小到大的顺序连接起来的**。

4. 记录的真实数据
  
记录的真实数据除了我们自己定义的列的数据以外，还会有三个隐藏列：

|列名          | 是否必须   |  占用空间    |  描述 |
|:----          | :---:    | :---        |:---|
|row_id| 38字节 |否         |6字节       |行ID, 唯一标识一条记录|
|transaction_id | 是        |6字节      | 事务ID|
|roll_pointer   |是         | 7字节     |回滚指针|
实际上这几个列的真正名称其实是：DB_ROW_ID、DB_TRX_ID、DB_ROLL_PTR。
- 一个表没有手动定义主键，则会选取一个Unique键作为主键，如果连Unique键都没有定义的话，则会为表默认添加一个名为row_id的隐藏列作为主键。所以**row_id是在没有自定义主键以及Unique键的情况下才会存在的**。
- 事务ID和回滚指针在后面的《第14章_MySQL事务日志》章节中讲解。

**Dynamic和Compressed行格式**

InnoDB存储引擎可以**将一条记录中的某些数据存储在真正的数据页面之外**。我们可以知道一个页的大小一般是16KB，也就是16384字节，而一个VARCHAR(M)类型的列就最多可以存储65533个字节，这样就可能出现一个页存放不了一条记录，这种现象称为**行溢出**。 
在Compact和Reduntant行格式中，对于占用存储空间非常大的列，在记录的真实数据处只会存储该列的一部分数据，把剩余的数据分散存储在几个其他的页中进行分页存储，然后记录的真实数据处用20个字节存储指向这些页的地址（当然这20个字节中还包括这些分散在其他页面中的数据的占用的字节数），从而可以找到剩余数据所在的页。

在MySQL 8.0中，默认行格式就是Dynamic，Dynamic、Compressed行格式和Compact行格式挺像，只不过在处理行溢出数据时有分歧：
- Compressed和Dynamic两种记录格式对于存放在BLOB中的数据采用了完全的行溢出的方式。如图，在数据页中只存放20个字节的指针（溢出页的地址），实际的数据都存放在Off Page（溢出页）中。
- Compact和Redundant两种格式会在记录的真实数据处存储一部分数据（存放768个前缀字节）。

**Redundant行格式**
Redundant是MySQL 5.0版本之前InnoDB的行记录存储方式，MySQL 5.0支持Redundant是为了兼容之前版本的页格式。
不同于Compact行记录格式，Redundant行格式的首部是一个**字段长度偏移列表**（为该条记录的所有字段），同样是按照列的顺序**逆序放置**的。
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_redundant.png"/>

### 索引的创建与设计原则
#### 索引的声明和使用

**索引的分类**
MySQL的索引包括普通索引、唯一性索引、全文索引、单列索引、多列索引和空间索引（只有MylSAM存储引擎支持空间检索）等。
- 从功能逻辑 上说，索引主要有 4 种，分别是**普通索引、唯一索引、主键索引、全文索引**。
  - 普通索引：在创建普通索引时，不附加任何限制条件，只是用于提高查询效率。这类索引可以创建在任何数据类型中，其值是否唯一和非空，要由字段本身的完整性约束条件决定。
  - 唯一索引：使用UNIQUE参数可以设置索引为唯一性索引，在创建唯一性索引时，限制该索引的值必须是唯一的，但允许有空值。在一张数据表里可以有多个唯一索引。
  - 主键索引：NOTNULL+UNIQUE,一张表里最多只有一个主键索引。
  - 全文索引：全文索引(也称全文检索)是目前搜索引擎使用的一种关键技术。它能够利用【分词技术】等多种算法智能分析出文本文字中关键词的频率和重要性，然后按照一定的算法规则智能地筛选出我们想要的搜索结果。**全文索引非常适合大型数据集**，对于小的数据集，它的用处比较小。全文索引典型的有两种类型:**自然语言的全文索引和布尔全文索引**。
- 按照物理实现方式 ，索引可以分为 2 种：聚簇索引和非聚簇索引。
- 按照作用字段个数进行划分，分成单列索引和联合索引。
  - 单列索引：在表中的单个字段上创建索引。单列索引只根据该字段进行索引。单列索引可以是普通索引，也可以是唯一性索引，还可以是全文索引。只要保证该索引只对应一个字段即可。一个表可以有**多个单列索引**。 
  - 多列(组合、联合)索引：多列索引是在表的多个字段组合上创建一个索引。

小结：不同的存储引擎支持的索引类型也不一样
- InnoDB ：支持 B-tree、Full-text 等索引，不支持 Hash索引；
- MyISAM ： 支持 B-tree、Full-text 等索引，不支持 Hash 索引
- Memory ：支持 B-tree、Hash 等索引，不支持 Full-text 索引；
- NDB ：支持 Hash 索引，不支持 B-tree、Full-text 等索引；
- Archive ：不支持 B-tree、Hash、Full-text 等索引；

**创建索引**
MySQL支持多种方法在单个或多个列上创建索引:在创建表的定义语句``CREATE TABLE``中指定索引列，使用``ALTER TABLE``语句在存在的表上创建索引，或者使用``CREATE INDEX``语句在已存在的表上添加索引。

创建表的时候创建索引：
1. 隐式方式：在声明有**主键约束、唯一性约束、外键约束**的字段上，会自动的添加相关的索引
2. 显示方式：基本语法格式如下：
    ```sql
    CREATE TABLE table_name [col_name data_type]
    [UNIQUE | FULLTEXT | SPATIAL] [INDEX | KEY] [index_name] (col_name [length]) [ASC |DESC]
    ```
    - UNIQUE 、 FULLTEXT 和 SPATIAL 为可选参数，分别表示唯一索引、全文索引和空间索引；
    - INDEX 与 KEY 为同义词，两者的作用相同，用来指定创建索
    - index_name 指定索引的名称，为可选参数，如果不指定，那么MySQL默认col_name为索引名；
    - col_name 为需要创建索引的字段列，该列必须从数据表中定义的多个列中选择；
    - length 为可选参数，表示索引的长度，只有字符串类型的字段才能指定索引长度；
    - ASC 或 DESC 指定升序或者降序的索引值存储。
    ```sql
    #① 创建普通的索引
    CREATE TABLE book(
    book_id INT ,
    book_name VARCHAR(100),
    AUTHORS VARCHAR(100),
    info VARCHAR(100) ,
    COMMENT VARCHAR(100),
    year_publication YEAR,
    #声明索引
    INDEX idx_bname(book_name)
    );
    #通过命令查看索引
    #方式1：
    SHOW CREATE TABLE book;
    #方式2：
    SHOW INDEX FROM book;

    #② 创建唯一索引
    # 声明有唯一索引的字段，在添加数据时，要保证唯一性，但是可以添加null
    CREATE TABLE book1(
    book_id INT ,
    book_name VARCHAR(100),
    AUTHORS VARCHAR(100),
    info VARCHAR(100) ,
    COMMENT VARCHAR(100),
    year_publication YEAR,
    #声明索引
    UNIQUE INDEX uk_idx_cmt(COMMENT)
    );

    #③ 主键索引
    #通过定义主键约束的方式定义主键索引
    CREATE TABLE book2(
    book_id INT PRIMARY KEY ,
    book_name VARCHAR(100),
    AUTHORS VARCHAR(100),
    info VARCHAR(100) ,
    COMMENT VARCHAR(100),
    year_publication YEAR
    );

    #通过删除主键约束的方式删除主键索引
    ALTER TABLE book2
    DROP PRIMARY KEY;
    ```

在已经存在的表上创建索引：在已经存在的表中创建索引可以使用ALTER TABLE语句或者CREATE INDEX语句。
1. **使用ALTER TABLE语句创建索引** ALTER TABLE语句创建索引的基本语法如下：
    ```sql
    ALTER TABLE table_name ADD [UNIQUE | FULLTEXT | SPATIAL]  
    [INDEX | KEY] [index_name] (col_name[length],...) [ASC | DESC]
    ```
2. **使用CREATE INDEX创建索引** CREATE INDEX语句可以在已经存在的表上添加索引，在MySQL中，CREATE INDEX被映射到一个ALTER TABLE语句上，基本语法结构为：
    ```sql
    CREATE [UNIQUE | FULLTEXT | SPATIAL] INDEX index_name 
    ON table_name (col_name[length],...) [ASC | DESC]
    ```
    举例：
    ```sql
    #① ALTER TABLE ... ADD ...
    CREATE TABLE book5(
    book_id INT ,
    book_name VARCHAR(100),
    AUTHORS VARCHAR(100),
    info VARCHAR(100) ,
    COMMENT VARCHAR(100),
    year_publication YEAR
    );
    ALTER TABLE book5 ADD INDEX idx_cmt(COMMENT);

    #② CREATE INDEX ... ON ...
    CREATE TABLE book6(
    book_id INT ,
    book_name VARCHAR(100),
    AUTHORS VARCHAR(100),
    info VARCHAR(100) ,
    COMMENT VARCHAR(100),
    year_publication YEAR
    );
    CREATE INDEX idx_cmt ON book6(COMMENT);
    ```
**删除索引**
1. 使用ALTER TABLE删除索引 ALTER TABLE删除索引的基本语法格式如下：
    ```sql
    ALTER TABLE table_name DROP INDEX index_name;
    ```
2. 使用DROP INDEX语句删除索引 DROP INDEX删除索引的基本语法格式如下：
    ```sql
    DROP INDEX index_name ON table_name;
    ```
    特殊地，可以删除联合索引的某个字段：
    ```sql
    ALTER TABLE book5
    DROP COLUMN book_name;
    ```
#### MySQL8.0索引新特性
**支持降序索引**: 
```sql
CREATE TABLE ts1(a int,b int,index idx_a_b(a,b desc));
```

**隐藏索引**
从MySQL 8.x开始支持**隐藏索引**（invisible indexes），只需要将待删除的索引设置为隐藏索引，使查询优化器不再使用这个索引（即使使用force index（强制使用索引），优化器也不会使用该索引），确认将索引设置为隐藏索引后系统不受任何响应，就可以彻底删除索引。**这种通过先将索引设置为隐藏索引，再删除索引的方式就是软删除**。
```sql
#① 创建表时，隐藏索引
CREATE TABLE book7(
book_id INT ,
book_name VARCHAR(100),
AUTHORS VARCHAR(100),
info VARCHAR(100) ,
COMMENT VARCHAR(100),
year_publication YEAR,
#创建不可见的索引
INDEX idx_cmt(COMMENT) invisible
);
SHOW INDEX FROM book7;


#② 创建表以后
ALTER TABLE book7 ADD UNIQUE INDEX uk_idx_bname(book_name) invisible;
CREATE INDEX idx_year_pub ON book7(year_publication); # 默认可见

#修改索引的可见性
ALTER TABLE book7 ALTER INDEX idx_year_pub invisible; #可见--->不可见
ALTER TABLE book7 ALTER INDEX idx_cmt visible; #不可见 ---> 可见
```

#### 索引的设计原则
**哪些情况适合创建索引**
1. 字段的数值有唯一性的限制
    > 业务上具有唯一特性的字段，即使是组合字段，也必须建成唯一索引。（来源：Alibaba）
    > 说明：不要以为唯一索引影响了 insert 速度，这个速度损耗可以忽略，但提高查找速度是明显的。
2. 频繁作为 WHERE 查询条件的字段
某个字段在SELECT语句的 WHERE 条件中经常被使用到，那么就需要给这个字段创建索引了。尤其是在数据量大的情况下，创建普通索引就可以大幅提升数据查询的效率。
3. 经常 GROUP BY 和 ORDER BY 的列
    ```sql
    #student_id字段上有索引的：
    SELECT student_id, COUNT(*) AS num 
    FROM student_info 
    GROUP BY student_id LIMIT 100; #41ms

    #删除idx_sid索引
    DROP INDEX idx_sid ON student_info;


    #student_id字段上没有索引的：
    SELECT student_id, COUNT(*) AS num 
    FROM student_info 
    GROUP BY student_id LIMIT 100; #866ms
    ```
4. UPDATE、DELETE 的 WHERE 条件列
对数据按照某个条件进行查询后再进行 UPDATE 或 DELETE 的操作，如果对 WHERE 字段创建了索引，就能大幅提升效率。

    ```sql
    UPDATE student_info SET student_id = 10002 
    WHERE NAME = '462eed7ac6e791292a79';  #0.633s
    #添加索引
    ALTER TABLE student_info
    ADD INDEX idx_name(NAME);
    UPDATE student_info SET student_id = 10001 
    WHERE NAME = '462eed7ac6e791292a79'; #0.001s
    ```
5. DISTINCT 字段需要创建索引
有时候我们需要对某个字段进行去重，使用 DISTINCT，那么对这个字段创建索引，也会提升查询效率。
6. 多表 JOIN 连接操作时，创建索引注意事项
- 首先，`连接表的数量尽量不要超过 3 张`，因为每增加一张表就相当于增加了一次嵌套的循环，数量级增长会非常快，严重影响查询的效率。
- 其次，`对 WHERE 条件创建索引`，因为 WHERE 才是对数据条件的过滤。如果在数据量非常大的情况下，没有 WHERE 条件过滤是非常可怕的。
- 最后，`对用于连接的字段创建索引`，并且该字段在多张表中的`类型必须一致`。比如 course_id 在 student_info 表和 course 表中都为 int(11) 类型，而不能一个为 int 另一个为 varchar 类型。
  
7. 使用列的类型小的创建索引
我们这里所说的类型大小指的就是该类型表示的数据范围的大小。数据类型越小，在查询时进行的比较操作越快。
8. 使用字符串前缀创建索引
通过截取字段的前面一部分内容建立索引，这个就叫前缀索引。
    > 拓展：Alibaba《Java开发手册》
    > 【 强制 】在 varchar 字段上建立索引时，必须指定索引长度，没必要对全字段建立索引，根据实际文本区分度决定索引长度。
    > 说明：索引的长度与区分度是一对矛盾体，一般对字符串类型数据，长度为 20 的索引，区分度会 高达 90% 以上 ，可以使用 count(distinct left(列名, 索引长度))/count(*)的区分度来确定。
9. 区分度高(散列性高)的列适合作为索引
10. 使用最频繁的列放到联合索引的左侧(最左)
11. 在多个字段都要创建索引的情况下，联合索引优于单值索引

**限制索引的数目**
在实际工作中，我们也需要注意平衡，索引的数目不是越多越好。我们需要限制每张表上的索引数量，建议单张表索引数量**不超过6个**。原因:
- 每个索引都需要占用磁盘空间，索引越多，需要的磁盘空间就越大。
- 索引会影响INSERT、DELETE、UPDATE等语句的性能，因为表中的数据更改的同时，索引也会进行调整和更新，会造成负担。
- 优化器在选择如何优化查询时，会根据统一信息，对每一个可以用到的索引来进行评估，以生成出一个最好的执行计划，如果同时有很多个索引都可以用于查询，会增加MysQL优化器生成执行计划时间，降低查询性能。

**哪些情况不适合创建索引**
1. 在where中使用不到的字段，不要设置索引
2. 数据量小的表最好不要使用索引
3. 有大量重复数据的列上不要建立索引
结论：当数据重复度大，比如`高于 10% `的时候，也不需要对这个字段使用索引。索引的价值是帮你快速定位。如果想要定位的数据有很多，那么索引就失去了它的使用价值，比如通常情况下的性别字段。
4. 避免对经常更新的表创建过多的索引
5. 不建议用无序的值作为索引
6. 删除不再使用或者很少使用的索引
7. 不要定义冗余或重复的索引

**小结**：索引是一把双刃剑，可提高查询效率，但也会降低插入和更新的速度并占用磁盘空间。选择索引的最终目的是为了使查询的速度变快，上面给出的原则是最基本的准则，但不能拘泥于上面的准则,大家要在以后的学习和工作中进行不断的实践，根据应用的实际情况进行分析和判断，选择最合适的索引方式。

### 性能分析工具的使用
<h4 align="left">数据库服务器的优化步骤</h4>

整个流程划分成了**观察** （Show status） 和 **行动**（Action）两个部分。字母 S 的部分代表观察（会使用相应的分析工具），字母 A 代表的部分是行动（对应分析可以采取的行动）。

<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_performance_analysis.png"/>

如果我们发现执行SQL时存在不规则延迟或卡顿的时候，就可以采用分析工具帮我们定位有问题的SQL，这三种分析工具你可以理解是SQL调优的三个步骤: **慢查询、EXPLAIN和SHOW PROFILING**。
**小结：**
<img src="https://images--hosting.oss-cn-chengdu.aliyuncs.com/db/index_performance_analysis2.png"/>

<h4 align="left">查看系统性能参数</h4>

SHOW STATUS语句语法如下：
```sql
SHOW [GLOBAL|SESSION] STATUS LIKE '参数';
```
一些常用的性能参数如下：
- Connections：连接MySQL服务器的次数。
- Uptime：MySQL服务器的上线时间。
- Slow_queries：慢查询的次数。
- Innodb_rows_read：Select查询返回的行数
- Innodb_rows_inserted：执行INSERT操作插入的行数
- Innodb_rows_updated：执行UPDATE操作更新的行数
- Innodb_rows_deleted：执行DELETE操作删除的行数 • Com_select：查询操作的次数。
- Com_insert：插入操作的次数。对于批量插入的 INSERT 操作，只累加一次。
- Com_update：更新操作的次数。
- Com_delete：删除操作的次数。

<h4 align="left"> 统计SQL的查询成本：last_query_cost </h4>

显示查询的页数
```sql
mysql> SHOW STATUS LIKE 'last_query_cost'; 
+-----------------+----------+
| Variable_name   |    Value | 
+-----------------+----------+
| Last_query_cost | 1.000000 | 
+-----------------+-----
```
> SQL查询是一个动态的过程，从页加载的角度来看，我们可以得到以下两点结论:
> - **位置决定效率**。如果页就在数据库缓冲池中，那么效率是最高的，否则还需要从内存或者磁盘中进行读取，当然针对单个页的读取来说，如果页存在于内存中，会比在磁盘中读取效率高很多。
> - **批量决定效率**。如果我们从磁盘中对单一页进行随机读，那么效率是很低的(差不多10ms)，而采用顺序读取的方式，批量对页进行读取，平均一页的读取效率就会提升很多，甚至要快于单个页面在内存中的随机读取。

<h4 align="left"> 定位执行慢的 SQL：慢查询日志 </h4>
MySQL的慢查询日志，用来记录在MySQL中响应时间超过阀值的语句，具体指运行时间超过long_query_time值的SQL，则会被记录到慢查询日志中。long_query_time的默认值为10，意思是运行10秒以上(不含10秒)的语句，认为是超出了我们的最大忍耐时间值。

默认情况下，MySQL数据库没有开启**慢查询日志**，需要我们手动来设置这个参数。**如果不是调优需要的话，一般不建议启动该参数**，因为开启慢查询日志会或多或少带来一定的性能影响。
1. 开启慢查询日志参数
- 开启slow_query_log
  查看慢查询是否已经开启：
  ```sql
  mysql>  show variables like '%slow_query_log';
  ```
  看到``slow_query_log=OFF``，我们可以把慢查询日志打开
  ```sql
  mysql> set global slow_query_log= ON;
  ```
- 修改long_query_time阈值
  ```sql
  mysql> show variables like '%long_query_time%';
  ```
  ```sql
  mysql> set global long_query_time = 1;  # or
  mysql> set long_query_time=1; 
  ```
2. 查看慢查询数目
  查询当前系统中有多少条慢查询记录
    ```sql
    SHOW GLOBAL STATUS LIKE '%Slow_queries%';
    ```
3. 慢查询日志分析工具：mysqldumpslow
分析慢查询日志的工具，mysqldumpslow 命令的具体参数如下：
- a: 不将数字抽象成N，字符串抽象成S
- s: 是表示按照何种方式排序：
- c: 访问次数
- l: 锁定时间
- r: 返回记录
- t: 查询时间
- al:平均锁定时间
- ar:平均返回记录数
- at:平均查询时间 （默认方式）
- ac:平均查询次数
- -t: 即为返回前面多少条的数据；
- -g: 后边搭配一个正则匹配模式，大小写不敏感的；

  举例：
  ```shell
  [root@localhost ~]# mysqldumpslow -s t -t 5 /var/lib/mysql/atguigu01-slow.log 
  ```
4. 关闭慢查询日志
- 永久性方式: 修改my.cnf或者my.ini文件，把[mysqld]组下的slow_query_log值设置为OFF，修改保存后，再重启MysQL服务,即可生效;或者，把slow_query_log一项注释掉 或 删除
- 临时性方式:
  ```sql
  SET GLOBAL slow_query_log=off;
  ``` 
5. 删除慢查询日志
    使用SHOW语句显示慢查询日志信息:
    ```sql
    SHow VARIABLES LIKE 'slow_query_log%';
    ```
    从执行结果可以看出，慢查询日志的目录默认为MySQL的数据目录，在该目录下手动删除慢查询日志文件即可。使用命令mysqladmin flush-logs来重新生成查询日志文件，具体命令如下，执行完毕会在数据目录下重新生成慢查询日志文件。
    ```sql
    mysqladmin -uroot -p flush-logs slow
    ```
    > 慢查询日志都是使用mysqladmin flush-logs命令来删除重建的。使用时一定要注意，一旦执行了这个命令，慢查询日志都只存在新的日志文件中，如果需要旧的查询日志，就必须事先备份。

<h4 align="left"> 查看 SQL 执行成本：SHOW PROFILE </h4>
Show profile是MysQL提供的可以用来分析当前会话中SQL都做了什么、执行的资源消耗情况的工具，可用于sql调优的测量。默认情况下处于关闭状态，并保存最近15次的运行结果。

```sql
mysql> show variables like 'profiling';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| profiling     | OFF   |
+---------------+-------+
1 row in set (0.00 sec)
```
通过设置 profiling='ON’ 来开启 show profile：
```sql
mysql > set profiling = 'ON';
```

然后执行相关的查询语句。接着看下当前会话都有哪些 profiles，使用下面这条命令:
```sql
mysql> show profiles;
```
查看指定的Query lD的开销
```sql
mysql> show profile; # show profile for 1
mysql> show profile for 2; # show profile for 1
```
查看不同部分开销
```sql
mysql> show profile cpu,block io for query 2;
```
show profile的常用查询参数：
- ALL：显示所有的开销信息.
- BLOCK IO：显示块IO开销。 
- CONTEXT SWITCHES：上下文切换开
销。 
- CPU：显示CPU开销信息。
- IPC：显示发送和接收开销信息。
- MEMORY：显示内存开销信息。
- PAGE FAULTS：显示页面错误开销信息。
- SOURCE：显示和Source_function，Source_file， Source_line相关的开销信息。
- SWAPS：显示交换次数开销信息。

> 不过SHOW PROFILE命令将被弃用，我们可以从information_schema中的profiling数据表进行查看。

<h4 align="left"> 分析查询语句：EXPLAIN </h4>

定位了查询慢的SQL之后，我们就可以使用EXPLAIN或DESCRIBE工具做针对性的分析查询语句。MySQL为我们提供了EXPLAIN语句来帮助我们查看某个查询语句的具体**执行计划**。

```sql
mysql> EXPLAIN SELECT * FROM student_info;
+----+-------------+--------------+------------+------+---------------+------+---------+------+--------+----------+-------+
| id | select_type | table        | partitions | type | possible_keys | key  | key_len | ref  | rows   | filtered | Extra |
+----+-------------+--------------+------------+------+---------------+------+---------+------+--------+----------+-------+
|  1 | SIMPLE      | student_info | NULL       | ALL  | NULL          | NULL | NULL    | NULL | 997458 |   100.00 | NULL  |
+----+-------------+--------------+------------+------+---------------+------+---------+------+--------+----------+-------+
1 row in set, 1 warning (0.00 sec)
```
输出的上述信息就是所谓的**执行计划**。在这个执行计划的辅助下，我们需要知道应该怎样改进自己的查询语句以使查询执行起来更高效。其实除了以SELECT开头的查询语句，其余的**DELETE、INSERT、REPLACE以及UPDATE**语句等都可以加上EXPLAIN，用来查看这些语句的执行计划，只是平时我们对SELECT语句更感兴趣。

> 注意:执行EXPLAIN时并没有真正的执行该后面的语句，因此可以安全的查看执行计划。

**EXPLAIN各列作用**
1. table: 表名
2. id: 在一个大的查询语句中每个 SELECT 关键字都对应一个唯一的 id。 **有几个表就有几行记录， 有几个SELECT就有几个不同的id**（这是是种类格式而不是总个数）
小结:
   - id如果相同，可以认为是一组，从上往下顺序执行
   - 在所有组中，id值越大，优先级越高，越先执行
   - 关注点：id号每个号码，表示一趟独立的查询, 一个sql的查询趟数越少越好
3. select_type：SELECT 关键字对应的那个查询的类型。MySQL为每一个SELECT关键字代表的小查询都定义了一个称之为**select_type**的属性，意思是我们只要知道了某个小查询的**select_type属性**，就知道了这个**小查询在整个大查询中扮演了一个什么角色**。
- SIMPLE：查询语句中不包含UNION或者子查询的查询都算作是SIMPLE类型
- PRIMARY：对于包含UNION、UNION ALL或者子查询的大查询来说，它是由几个小查询组成的，其中最左边的那个查询的select_type值就是PRIMARY
- UNION：对于包含UNION或者UNIN ALL的大查询来说，它是由几个小查询组成的，其中除了最左边的那个小查询以外，其余的小查询的select_type值就是UNION
- UNION RESULT：MySQL选择使用临时表来完成UNION查询的去重工作，针对该临时表的查询的select_type就是UNION RESULT
- SUBQUERY：如果包含子查询的查询语句不能够转为对应的semi-join的形式，并且该子查询是不相关子查询，并且查询优化器决定采用将该子查询物化的方案来执行该子查询时，该子查询的第一个SELECT关键字代表的那个查询的select_type就是 SUBQUERY
- ......
4. type(重要)：	针对单表的访问方法。
  完整的访问方法如下： ``system, const, eq_ref, ref, fulltext, ref_or_null, index_merge, unique_subquery, index_subquery, range, index, ALL``(越前面越好)。
  我们详细解释一下：

- system: 当表中只有一条记录并且该表使用的存储引擎的统计数据是精确的，比如MyISAM、Memory，那么对该表的访问方法就是system
- const： 当我们根据主键或者唯一二级索引列与常数进行等值匹配时，对单表的访问方法就是const
- eq_ref： 在连接查询时，如果被驱动表是通过主键或者唯一二级索引列等值匹配的方式进行访问的(如果该主键或者唯一二级索引是联合索引的话，所有的索引列都必须进行等值比较)，则对该被驱动表的访问方法就是eq_ref
- ref：当通过普通的二级索引列与常量进行等值匹配时来查询某个表，那么对该表的访问方法就可能是ref
- fulltext：全文索引
- ref_or_null：当对普通二级索引进行等值匹配查询，该索引列的值也可以是NULL值时，那么对该表的访问方法就可能是ref_or_null
- index_merge：一般情况下对于某个表的查询只能使用到一个索引，但单表访问方法时在某些场景下可以使用Intersection、Union、Sort-Union这三种索引合并的方式来执行查询。
    ```sql
    mysql> EXPLAIN SELECT * FROM s1 WHERE key1 = 'a' OR key3 = 'a';
    ```
- unique_subquery: 类似于两表连接中被驱动表的eq_ref访问方法，unique_subquery是针对在一些包含IN子查询的查询语句中，如果查询优化器决定将IN子查询转换为EXISTS子查询，而且子查询可以使用到主键进行等值匹配的话，那么该子查询执行计划的type列的值就是unique_subquery
- index_subquery: index_subquery与unique_subquery类似，只不过访问子查询中的表时使用的是普通的索引
- range: 如果使用索引获取某些范围区间的记录，那么就可能使用到range访问方法
- index: 当我们可以使用索引覆盖，但需要扫描全部的索引记录时，该表的访问方法就是index
- ALL: 最熟悉的全表扫描

  >小结(阿里巴巴开发手册要求): 结果值从最好到最坏依次是: system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery \( > \) index_subquery \( > \) range \( > \) index \( > \) ALL 其中比较重要的几个提取出来 (见上图中的蓝 色) 。SQL 性能优化的目标: 至少要达到 range 级别，要求是 ref 级别，最好是 consts级别。（
5. possible_keys和key: 在EXPLAIN语句输出的执行计划中,**possible_keys**列表示在某个查询语句中，对某个表执行**单表查询时可能用到的索引**有哪些。一般查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询使用。**key**列表示**实际用到的索引**有哪些，如果为NULL，则没有使用索引。
6. key_len: 使用的索引长度（字节数）：帮你检查是否充分利用了索引，值越大越好，主要针对联合索引，有一定参考意义。
7.  ref：当我们使用索引列等值查询时，与索引列进行等值匹配的对象信息
8.  rows: 预估的需要读取的记录条数（值越小越好）。
9.  filtered: 某个表经过搜索条件过滤后剩余记录条数的百分比（越高越好）。对于单表查询来说，这个filtered列的值没什么意义，我们**更关注在连接查询中驱动表对应的执行计划记录的filtered值**，它决定了被驱动表要执行的次数(即: rows * filtered)。
10. Extra(重要): 顾名思义，Extra列是用来说明一些额外信息的，包含不适合在其他列中显示但十分重要的额外信息。我们可以通过这些额外信息来**更准确的理解MySQL到底将如何执行给定的查询语句**。MySQL提供的额外信息有好几十个。如：No tables used, Impossible WHERE, Using where, No matching min/max row, Using index, Using index condition, Using join buffer (Block Nested Loop)等。

**小结：**
- EXPLAIN不考虑各种Cache
- EXPLAIN不能显示MySQL在执行查询时所作的优化工作
- EXPLAIN不会告诉你关于触发器、存储过程的信息或用户自定义函数对查询的影响情况
- 部分统计信息是估算的，并非精确值

<h4 align="left"> EXPLAIN的进一步使用 </h4>

1.  EXPLAIN四种输出格式
这里谈谈EXPLAIN的输出格式。EXPLAIN可以输出四种格式： **传统格式，JSON格式 ，TREE格式以及可视化输出**。用户可以根据需要选择适用于自己的格式。
- 传统格式： 输出是一个表格形式，前面用的都是传统格式。
- JSON格式： JSON格式：在EXPLAIN单词和真正的查询语句中间加上 FORMAT=JSON 
  ```sql
  EXPLAIN FORMAT=JSON SELECT ....
  ```
- TREE格式: 主要根据查询的**各个部分之间的关系**和**各部分的执行顺序**来描
述如何查询。
  ```sql
  EXPLAIN FORMAT=tree SELECT ....
  ```
- 可视化输出: 通过MySQL Workbench可视化查看MySQL的执行计划
2. SHOW WARNINGS的使用: 在我们使用EXPLAIN语句查看了某个查询的执行计划后，紧接着还可以使用``SHOW WARNINGS``语句查看与这个查询的执行计划有关的一些扩展信息。
  
<h4 align="left"> 分析优化器执行计划：trace </h4>

**OPTIMIZER_TRACE** 是MySQL 5.6引入的一项跟踪功能，它可以跟踪优化器做出的各种决策（比如访问表的方法、 各种开销计算、各种转换等)，并将跟踪结果记录到**INFORMATION_SCHEMA.OPTIMIZER_TRACE** 表中。

此功能默认关闭。开启trace，并设置格式为 JSON，同时设置trace最大能够使用的内存大小，避免解析过程中因 为默认内存过小而不能够完整展示。
```sql
SET optimizer_trace="enabled=on", end_markers_in_json=on;
set optimizer_trace_max_mem_size=1000000;
```
开启后，可分析如下语句：SELECT, INSERT, REPLACE, UPDATE, DELETE, EXPLAIN, SET, DECLARE, CASE, IFRETURN, CALL。
 
查询 information_schema.optimizer_trace 就可以知道MySQL是如何执行SQL的 ：
```sql
select * from information_schema.optimizer_trace\G
```

<h4 align="left"> MySQL监控分析视图-sys schema </h4>
sys schema，它将performance_schema和information_schema中的数据以更容易理解的方式兰峙归纳为"视 图”，其目的就是为了降低查询performance_schema的复杂度。

Sys schema视图摘要：
- 主机相关：以host_summary开头，主要汇总了IO延迟的信息。
- Innodb相关：以innodb开头，汇总了innodb buffer信息和事务等待innodb锁的信息。
- I/o相关：以io开头，汇总了等待I/O、I/O使用量情况。
- 内存使用情况：以memory开头，从主机、线程、事件等角度展示内存的使用情况
- 连接与会话信息：processlist和session相关视图，总结了会话相关信息。
- 表相关：以schema_table开头的视图，展示了表的统计信息。
- 索引信息：统计了索引的使用情况，包含冗余索引和未使用的索引情况。
- 语句相关：以statement开头，包含执行全表扫描、使用临时表、排序等的语句信息。
- 用户相关：以user开头的视图，统计了用户使用的文件I/O、执行语句统计信息。
- 等待事件相关信息：以wait开头，展示等待事件的延迟情况

Sys schema视图使用场景：
1. 索引情况：
    ```sql
    #1. 查询冗余索引 
    select * from sys.schema_redundant_indexes; 
    #2. 查询未使用过的索引 select * from sys.schema_unused_indexes; 
    #3. 查询索引的使用情况 select index_name,rows_selected,rows_inserted,rows_updated,rows_deleted
    from sys.schema_index_statistics where table_schema='dbname' ;
    ```
2. 表相关：
    ```sql
    # 1. 查询表的访问量 
    select table_schema,table_name,sum(io_read_requests+io_write_requests) as io from
    sys.schema_table_statistics group by table_schema,table_name order by io desc; 

    # 2. 查询占用bufferpool较多的表 
    select object_schema,object_name,allocated,data
    from sys.innodb_buffer_stats_by_table order by allocated limit 10; 

    # 3. 查看表的全表扫描情况 
    select * from sys.statements_with_full_table_scans where db='dbname';
    ```
3. 语句相关：
    ```sql
    #1. 监控SQL执行的频率 
    select db,exec_count,query from sys.statement_analysis order by exec_count desc; 

    #2. 监控使用了排序的SQL 
    select db,exec_count,first_seen,last_seen,query
    from sys.statements_with_sorting limit 1; 

    #3. 监控使用了临时表或者磁盘临时表的SQL 
    select db,exec_count,tmp_tables,tmp_disk_tables,query
    from sys.statement_analysis where tmp_tables>0 or tmp_disk_tables >0 order by
    (tmp_tables+tmp_disk_tables) desc;
    ```
4. IO相关：
    ```sql
    #1. 查看消耗磁盘IO的文件 
    select file,avg_read,avg_write,avg_read+avg_write as avg_io
    from sys.io_global_by_file_by_bytes order by avg_read limit 10;
    ```
5. Innodb 相关： 
    ```sql
    #1. 行锁阻塞情况 
    select * from sys.innodb_lock_waits;
    ```

> 通过sys库去查询时，MySQL会消耗大量资源去收集相关信息，严重的可能会导致业务请求鿆阻塞，从而引起故障。建议生产上不要频繁的去查询sys或者performance_schema、information_schema来完成监控、巡检等工作。

### 索引优化与查询优化 

#### 索引失效案例
索引提高访问和查询的效率，用不用索引，最终都是优化器说了算。优化器是基于**cost开销**(CostBaseOptimizer)，它不是基于规则(Rule-BasedOptimizer)，也不是基于语义。怎么样开销小就怎么来。另外，**SQL语句是否使用索引，跟数据库版本、数据量、数据选择度都有关系**。

- <h5 align="left"> 全值匹配我最爱 </h5>

  ```sql
  EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE age=30 and classId=4 AND name = 'abcd' ;
  CREATE INDEX idx_age_classid_name ON student( age ,classId , name ) ;
  ```

- <h5 align="left"> 最佳左前缀法则 </h5>

  在MySQL建立联合索引时会遵守最佳左前缀匹配原则，即最左优先，在检索数据时从联合索引的最左边开始匹配。

  结论:MySQL可以为多个字段创建索引，一个索引可以包括16个字段。对于多列索引，过滤条件要使用索引必须按照索引建立时的顺序，依次满足，**一旦跳过某个字段，索引后面的字段都无法被使用**。如果查询条件中没有使用这些字段中第1个字段时，多列(或联合)索引不会被使用。

- <h5 align="left"> 主键插入顺序 </h5>

  建议：**让主键具有 AUTO_INCREMENT**，让存储引擎自己为表生成主键，而不是我们手动插入 

- <h5 align="left">  计算、函数、类型转换(自动或手动)导致索引失效 </h5>

  ```sql
  EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE student.name LIKE 'abc%'; # 生效
  EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE LEFT(student.name,3) = 'abc'; # 失效
  EXPLAIN SELECT SQL_NO_CACHE id, stuno, NAME FROM student WHERE stuno+1 = 900001; # 失效
  ```
  ```sql
  # 未使用到索引 
  EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name=123; # 发生了类型转换
  # 使用到索引 
  EXPLAIN SELECT SQL_NO_CACHE * FROM student WHERE name='123';
  ```

- <h5 align="left"> 范围条件右边的列索引失效 </h5>

  ```sql
  CREATE INDEX idx_age_classId_name ON student(age,classId,NAME);
  EXPLAIN SELECT SQL_NO_CACHE * FROM student 
  WHERE student.age=30 AND student.classId>20 AND student.name = 'abc' ; 
  EXPLAIN SELECT SQL_NO_CACHE * FROM student 
  WHERE student.age=30 AND student.name = 'abc' AND student.classId>20; 
  ```

- <h5 align="left"> 不等于(!= 或者<>)索引失效 </h5>

- <h5 align="left"> is null可以使用索引，is not null无法使用索引 </h5>

- <h5 align="left"> like以通配符%开头索引失效 </h5>

- <h5 align="left"> OR 前后存在非索引的列，索引失效 </h5>

- <h5 align="left"> 数据库和表的字符集统一使用utf8mb4 </h5>











#### 关联查询的优化
EXPLAIN进行分析：**表的排前面的是驱动表（主表）**， **表的排后面的是被驱动表（从表）**。

**左外连接**：无论为type.card还是book.card创建索引，一般情况type是主表，book是从表。为从表创建索引才能提高效率。
```sql
mysql> EXPLAIN SELECT SQL_NO_CACHE * FROM `type` LEFT JOIN book ON type.card = book.card;
+----+-------------+-------+------------+-------+---------------+------+---------+----------------------+------+----------+-------------+
| id | select_type | table | partitions | type  | possible_keys | key  | key_len | ref                  | rows | filtered | Extra       |
+----+-------------+-------+------------+-------+---------------+------+---------+----------------------+------+----------+-------------+
|  1 | SIMPLE      | type  | NULL       | index | NULL          | X    | 4       | NULL                 |   42 |   100.00 | Using index |
|  1 | SIMPLE      | book  | NULL       | ref   | Y             | Y    | 4       | atguigudb2.type.card |    1 |   100.00 | Using index |
+----+-------------+-------+------------+-------+---------------+------+---------+----------------------+------+----------+-------------+
2 rows in set, 2 warnings (0.00 sec)
```

**内连接**：查询优化器会自动选择哪个是驱动表，哪个是被驱动表。
- type.card和book.card都没有被创建索引，查询优化器可以决定谁作为驱动表，谁作为被驱动表出现的（小表驱动大表）
- 如果表的连接条件中只能有一个字段有索引，则有索引的字段所在的表会被作为被驱动表出现。(被驱动表有索引会提高效率)
- 在两个表的连接条件都存在索引的情况下，会选择小表作为驱动表。“小表驱动大表”

**join语句原理**
- Simple Nested-Loop Join(简单嵌套循环连接)：被驱动表没有索引，逐个查询
- Index Nested-Loop Join(索引嵌套循环连接)：要求被驱动表有索引
- Block Nested-Loop Join(块嵌套循环连接)：针对被驱动表没有索引的改进
- Join小结：
  - 整体效率比较:INLJ >BNLJ > SNLJ
  - 永远用小结果集驱动大结果集(其本质就是减少外层循环的数据数量) (小的度量单位指的是表行数*每行大小)
  - 为被驱动表匹配的条件增加索引(减少内层表的循环匹配次数)
  - 增大join buffer size的大小(一次缓存的数据越多，那么内层包的扫表次数就越少)
  - 减少驱动表不必要的字段查询(字段越少，join buffer所缓存的数据就越多)
- Hash Join：做**大数据集连接**时的常用方式，优化器使用两个表中较小(相对较小）的表利用Join Key在内存中建立散列表，然后扫描较大的表并探测**散列表**，找出与Hash表匹配的行。

#### 子查询优化
子查询的执行效率不高。原因：
- 执行子查询时，MySQL需要为内层查询语句的查询结果**建立一个临时表** ，然后外层查询语句从临时表中查询记录。查询完毕后，再**撤销这些临时表**。这样会消耗过多的CPU和IO资源，产生大量的慢查询。
- 子查询的结果集存储的临时表，不论是内存临时表还是磁盘临时表都**不会存在索引**，所以查询性能会受到一定的影响。
- 对于返回结果集比较大的子查询，其对查询性能的影响也就越大。

可以使用连接（JOIN）查询来替代子查询。
```sql
#创建班级表中班长的索引
CREATE INDEX idx_monitor ON class(monitor);
#查询班长的信息
EXPLAIN SELECT * FROM student stu1
WHERE stu1.`stuno` IN (
SELECT monitor
FROM class c
WHERE monitor IS NOT NULL
);
EXPLAIN SELECT stu1.* FROM student stu1 JOIN class c 
ON stu1.`stuno` = c.`monitor`
WHERE c.`monitor` IS NOT NULL;

#查询不为班长的学生信息
EXPLAIN SELECT SQL_NO_CACHE a.* 
FROM student a 
WHERE  a.stuno  NOT  IN (
			SELECT monitor FROM class b 
			WHERE monitor IS NOT NULL) 

EXPLAIN SELECT SQL_NO_CACHE a.*
FROM  student a LEFT OUTER JOIN class b 
ON a.stuno =b.monitor
WHERE b.monitor IS NULL;
```

#### 排序优化
在 MySQL 中，支持两种排序方式，分别是**FileSort**和**Index**排序。
- Index 排序中，索引可以保证数据的有序性，不需要再进行排序，效率更高。
- FileSort 排序则一般在内存中进行排序，占用 CPU 较多。如果待排结果较大，会产生临时文件 I/O 到磁盘进行排序的情况，效率较低。

优化建议：
- SQL 中，可以在 WHERE 子句和 ORDER BY 子句中使用索引，目的是在 WHERE 子句中**避免全表扫**描 ，在 ORDER BY子句**避免使用FileSort**排序 。当然，某些情况下全表扫描，或者FileSort排序不一定比索引慢。但总的来说，我们还是要避免，以提高查询效率。
- 尽量使用 Index 完成 ORDER BY 排序。如果 WHERE 和 ORDER BY 后面是相同的列就使用单索引列；如果不同就使用联合索引。
- 无法使用 Index 时，需要对 FileSort 方式进行调优。

分析一些过程
- order by时不limit，索引失效
  ```sql
  #不限制,索引失效
  EXPLAIN  SELECT SQL_NO_CACHE * FROM student ORDER BY age,classid; 
  # 没有SELECT *, 使用上了索引
  EXPLAIN  SELECT SQL_NO_CACHE age,classid,name,id FROM student ORDER BY age,classid; 
  #增加limit过滤条件，使用上索引了。
  EXPLAIN  SELECT SQL_NO_CACHE * FROM student ORDER BY age,classid LIMIT 10;  
  ```
- order by时顺序错误，索引失效
- order by时规则不一致, 索引失效 （顺序错，不索引；方向反，不索引）
- 无过滤，不索引

#### GROUP BY优化和分页查询优化
- group by 使用索引的原则几乎跟order by一致 ，group by 即使没有过滤条件用到索引，也可以直接使用索引。
- group by 先排序再分组，遵照索引建的最佳左前缀法则
- 当无法使用索引列，增大 ``max_length_for_sort_data`` 和 ``sort_buffer_size`` 参数的设置
- where效率高于having，能写在where限定的条件就不要写在having中了
- 减少使用order by，和业务沟通能不排序就不排序，或将排序放到程序端去做。Order by、group by、distinct这些语句较为耗费CPU，数据库的CPU资源是极其宝贵的。
- 包含了order by、group by、distinct这些查询的语句，where条件过滤出来的结果集请保持在1000行以内，否则SQL会很慢。

#### 优化分页查询

一般分页查询时，通过创建覆盖索引能够比较好地提高性能。一个常见又非常头疼的问题就是 limit 2000000,10 ，此时需要MySQL排序前 2000010 记录，仅仅返回 \( 2000000-2000010 \) 的记录，其他记录丢弃，查询排序的代价非常大。
- 在索引上完成排序分页操作，最后根据主键关联回原表查询所需要的其他列内容。
  ```sql
  EXPLAIN SELECT * FROM student t,(SELECT id FROM student ORDER BY id LIMIT 2000000,10) a WHERE t.id = a.id;
  ```

- 该方案适用于主键自增的表，可以把Limit 查询转换成某个位置的查询 。
  ```sql
  EXPLAIN SELECT * FROM student WHERE id > 2000000 LIMIT 10;
  ```

#### 优先考虑覆盖索引

**理解方式一**：索引是高效找到行的一个方法，但是一般数据库也能使用索引找到一个列的数据，因此它不必读取整个行。毕竟索引叶子节点存储了它们索引的数据；当能通过读取索引就可以得到想要的数据，那就不需要读取行了。**一个索引包含了满足查询结果的数据就叫做覆盖索引**。

**理解方式二**：非聚簇复合索引的一种形式，它包括在查询里的SELECT、JOIN和WHERE子句用到的所有列（即建索引的字段正好是覆盖查询条件中所涉及的字段）。

简单说就是，**索引列+主键**包含**SELECT 到 FROM之间查询的列**。

**好处**：
- **避免Innodb表进行索引的二次查询（回表）**。
- **可以把随机IO变成顺序IO加快查询效率**

**弊端**：
索引字段的维护 总是有代价的。因此，在建立冗余索引来支持覆盖索引时就需要权衡考虑了。这是业务DBA，或者称为业务数据架构师的工作。

#### 索引下推
Index Condition Pushdown(ICP)是MySQL 5.6中新特性，是一种**在存储引擎层使用索引过滤数据的一种优化方式**。ICP可以减少存储引擎访问基表的次数以及MySQL服务器访问存储引擎的次数。

**使用前后对比**
Index Condition Pushdown(ICP)是MySQL 5.6中新特性，是一种在存储引擎层使用索引过滤数据的优化方式。
- 如果没有ICP，存储引擎会遍历索引以定位基表中的行，并将它们返回给MySQL服务器，由MySQL服务器评估WHERE后面的条件是否保留行。
- 启用ICP后，如果部分WHERE条件可以仅使用索引中的列进行筛选，则mysql服务器会把这部分WHERE条件放到存储引擎筛选。然后，存储引擎通过使用索引条目来筛选数据，并且只有在满足这一条件时才从表中读取行。
  - 好处: **ICP可以减少存储引擎必须访问基表的次数和MySQL服务器必须访问存储引擎的次数**。
  - 但是，**ICP的加速效果取决于在存储引擎内通过ICP筛选掉的数据的比例**。

#### 其它查询优化策略
<h5 align="left">EXISTS 和 IN 的区分</h5>

(小表驱动大表更高效)比如下面这样:
```sql
SELECT * FROM A WHERE cc IN (SELECT cc FROM B)
SELECT * FROM A WHERE EXISTS (SELECT cc FROM B WHERE B.cc=A.cc)
```
**当A小于B时, 用EXISTS**。因为EXISTS的实现，相当于外表循环，实现的逻辑类似于:
```
for i in A
	for j in B
		if j.cc == i.cc then
```
**当B小于A时, 用IN**，因为实现的逻辑类似于:
```
for i in B
	for j in A
		if j.cc == i.cc then ...
```

<h5 align="left"> COUNT(*)与COUNT(具体字段)效率 </h5>

- ``COUNT(*)``和``COUNT(1)``都是对所有结果进行COUNT，COUNT()和COUNT(1)本质上并没有区别(二者执行时间可能略有差别，不过你还是可以把它俩的执行效率看成是相等的)。如果有WHERE子句，则是对所有符合筛选条件的数据行进行统计;如果没有WHERE子句，则是对数据表的数据行数进行统计。 MYISAM O(1), InnoDB O(n)。

- 在InnoDB引擎中，如果采用``COUNT(具体字段)``来统计数据行数，要尽量采用二级索引。因为主键采用的索引是聚簇索引，聚簇索引包含的信息多，明显会大于二级索引(非聚簇索引)。对于COUNT(*)和COUNT(1)来说，它们不需要查找具体的行，只是统计行数，系统会自动采用占用空间更小的二级索引来进行统计。

<h5 align="left"> 关于SELECT(*) </h5>

在表查询中，建议明确字段，不要使用 * 作为查询的字段列表，推荐使用SELECT <字段列表> 查询。原因：
- MySQL 在解析的过程中，会通过**查询数据字典**将"*"按序转换成所有列名，这会大大的耗费资源和时间。
- 无法使用**覆盖索引**

<h5 align="left"> LIMIT 1 对优化的影响 </h5>

- 如果针对的是会扫描全表的 SQL 语句，如果你可以确定结果集只有一条，那么加上 LIMIT 1 的时候，当找到一条结果的时候就不会继续扫描了，这样会加快查询速度。
- 如果数据表已经对字段建立了唯一索引，那么可以通过索引进行查询，不会全表扫描的话，就不需要加上 LIMIT 1 了。

<h5 align="left">  多使用COMMIT </h5>

只要有可能，在程序中尽量多使用 COMMIT，这样程序的性能得到提高，需求也会因为 COMMIT 所释放的资源而减少。

COMMIT 所**释放**的资源：
- 回滚段上用于恢复数据的信息
- 被程序语句获得的锁
- redo / undo log buffer 中的空间
- 管理上述 3 种资源中的内部花费


### 数据库的设计规范
我们在设计数据表的时候，要考虑很多问题。比如:
- 用户都需要什么数据?需要在数据表中保存哪些数据?
- 如何保证数据表中数据的**正确性**，当插入、删除、更新的时候该进行怎样的**约束检查**?
- 如何降低数据表的**数据冗余度**，保证数据表不会因为用户量的增长而迅速扩张?
- 如何让负责数据库维护的人员更方便地使用数据库?
- 使用数据库的应用场景也各不相同，可以说针对不同的情况，设计出来的数据表可能千差万别。

#### 范式
**在关系型数据库中，关于数据表设计的基本原则、规则就称为范式**（Normal Form, NF）。可以理解为，一张数据表的设计结构需要满足的某种设计标准的级别。要想设计一个结构合理的关系型数据库，必须满足一定的范式。范式是关系数据库理论的基础，也是我们在设计数据库结构过程中所要遵循的规则和指导方法。

目前关系型数据库有六种常见范式，按照范式级别，从低到高分别是：**第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，又称完美范式）**。数据库的范式设计越高阶，冗余度就越低，同时高阶的范式一定符合低阶范式的要求，满足最低要求的范式是第一范式（1NF)。一般来说，在关系型数据库设计中，最高也就遵循到BCNF，普遍还是3NF。
![](images/index_normal_form.png)

 
<h5 align="left"> 键和相关属性的概念 </h5>

范式的定义会使用到主键和候选键，数据库中的键(Key)由一个或者多个属性组成。数据表中常用的几种键和属性的定义:

- **超键**: 能唯─标识元组的**属性集**叫做超键。
- **候选键**: 如果超键不包括多余的属性，那么这个超键就是候选键。
- **主键**: 用户可以从候选键中选择一个作为主键。
- **外键**∶如果数据表R1中的某属性集不是R1的主键，而是另一个数据表R2的主键，那么这个属性集就是数据表R1的外键。
- **主属性**: 包含在任一候选键中的属性称为主属性。
- **非主属性**: 与主属性相对，指的是不包含在任何一个候选键中的属性。

<h5 align="left"> 第一范式(1st NF) </h5>

第一范式主要是确保数据表中每个字段的值必须具有**原子性**，也就是说数据表中每个字段的值为不可再次拆分的**最小数据单元**。

<h5 align="left"> 第二范式(2nd NF) </h5>

第二范式要求，在满足第一范式的基础上，还要满足**数据表里的每一条数据记录，都是可唯一标识的。而且所有非主键字段，都必须完全依赖主键，不能只依赖主键的一部分**。如果知道主键的所有属性的值，就可以检索到任何元组(行)的任何属性的任何值。(要求中的主键，其实可以拓展替换为候选键)。

例子：比赛表 playe r_game ，里面包含球员编号、姓名、年龄、比赛编号、比赛时间和比赛场地等属性，这里候选键和主键都为（球员编号，比赛编号），我们可以通过候选键（或主键）来决定如下的关系：
```
(球员编号 ) → (姓名，年龄)
(比赛编号 ) → (比赛时间 , 比赛场地)
```
设计表满足第二范式
![](images/index_2nd_NF.png)

<h5 align="left"> 第三范式(3rd NF) </h5>

第三范式是在第二范式的基础上，确保数据表中的每一个非主键字段都和主键字段直接相关，也就是说，**要求数据表中的所有非主键字段不能依赖于其他非主键字段**。(即，不能存在非主属性A依赖于非主属性B，非主属性B依赖于主键C的情况，即存在“A→B一C”的决定关系）通俗地讲，该规则的意思是所有非主键属性之间不能有依赖关系，必须相互独立。

> 这里的主键可以拓展为候选键。

<h5 align="left"> 反范式化 </h5>

有的时候不能简单按照规范要求设计数据表，因为有的数据看似冗余，其实对业务来说十分重要。这个时候，我们就要遵循**业务优先**的原则，首先满足业务需求，再尽量减少冗余。

如果数据库中的数据量比较大，系统的UV和PV访问频次比较高，则完全按照MySQL的三大范式设计数据表，读数据时会产生大量的关联查询，在一定程度上会影响数据库的读性能。如果我们想对查询效率进行优化，**反范式优化**也是一种优化思路。此时，可以通过在数据表中**增加冗余字段**来提高数据库的读性能。

规范化 vs 性能:
- 为满足某种商业目标, 数据库性能比规范化数据库更重要
- 在数据规范化的同时, 要综合考虑数据库的性能
- 通过在给定的表中添加额外的字段，以大量减少需要从中搜索信息所需的时间
- 通过在给定的表中插入计算列，以方便查询

例子：员工的信息存储在 employees 表 中，部门信息存储在 departments 表 中。通过 employees 表中的department_id字段与 departments 表建立关联关系。如果要查询一个员工所在部门的名称：
```sql
select employee_id,department_name
from employees e join departments d 
on e.department_id = d.department_id;
```
如果经常需要进行这个操作，连接查询就会浪费很多时间。可以在 employees 表中增加一个冗余字段 department_name，这样就不用每次都进行连接操作了。

反范式的新问题：反范式可以通过空间换时间，提升查询的效率，但是反范式也会带来一些新问题:
- 存储**空间变大**了
- 一个表中字段做了修改，另一个表中冗余的字段也需要做同步修改，否则**数据不一致**
- 若采用存储过程来支持数据的更新、删除等额外操作，如果更新频繁，会非常**消耗系统资源**
- 在**数据量小**的情况下，反范式不能体现性能的优势，可能还会让数据库的设计更加复杂

反范式的适用场景：当冗余信息有价值或者能 大幅度提高查询效率 的时候，我们才会采取反范式的优化。
- 增加冗余字段的建议：
  - 这企冗余字段不需要经常进行修改;
  - 这个冗余字段查询的时候不可或缺。
- 历史快照、历史数据的需要
  - 反范式优化也常用在**数据仓库**的设计中，因为数据仓库通常 存储历史数据 ，对增删改的实时性要求不强，对历史数据的分析需求强。   

<h5 align="left"> BCNF(巴斯范式) </h5>

**巴斯范式 (BCNF)**被认为没有新的设计规范加入，只是对第三范式中设计规范要求更强，使得数据库冗余度更小。 所以，称为是**修正的第三范式**，或**扩充的第三范式**，BCNF不被称为第四范式。
若一个关系达到了第三范式，并且它只有一个候选键，或者它的每个候选键都是单属性，则该关系自然达到BCNF范式。
一般来说，一个数据库设计符合 3NF 或 BCNF就可以了。

#### ER模型
ER 模型也叫作**实体关系模型**，是用来描述现实生活中客观存在的事物、事物的属性，以及事物之间关系的一种数据模型。在开发基于数据库的信息系统的设计阶段，通常使用 ER 模型来描述信息需求和信息特性，帮助我们理清业务逻辑，从而设计出优秀的数据库。

**ER 模型中有三个要素**：
- **实体**: 可以看做是数据对象，往往对应于现实生活中的真实存在的个体。在 ER 模型中，用**矩形**来表示。实体分为两类，分别是**强实体**和**弱实体**。强实体是指不依赖于其他实体的实体；弱实体是指对另一个实体有很强的依赖关系的实体。
- **属性**，则是指实体的特性。比如超市的地址、联系电话、员工数等。在 ER 模型中用**椭圆形**来表示。
- **关系**，则是指实体之间的联系。比如超市把商品卖给顾客，就是一种超市与顾客之间的联系。在 ER 模型中用**菱形**来表示。

> 注意：实体和属性不容易区分。这里提供一个原则：我们要从系统整体的角度出发去看，**可以独立存在的是实体，不可再分的是属性**。也就是说，属性不能包含其他属性。

**关系的类型**：
- **一对一** ：指实体之间的关系是一一对应的。
- **一对多**：指一边的实体通过关系，可以对应多个另外一边的实体。
- **多对多**：指关系两边的实体都可以通过关系对应多个对方的实体。

给电商业务创建 ER 模型了，如图：
![](images/index_ER.png)
这个 ER 模型，包括了 8个实体之间的 8种关系，``用户``和``商品分类``是强实体。不同实体对应不同的属性。

**ER 模型图转换成数据表：**
转换的原则：
- 一个**实体**通常转换成一个**数据表**；
- 一个**多对多的关系**，通常也转换成一个**数据表**；
- 一个 **1 对 1，或者1对多**的关系，往往通过表的**外键**来表达，而不是设计一个新的数据表；
- **属性**转换成表的**字段**。

#### 数据表的设计原则
综合以上内容，总结出数据表设计的一般原则：“三少一多”
- 数据表的个数越少越好
- 数据表中的字段个数越少越好
- 数据表中联合主键的字段个数越少越好
- 使用主键和外键越多越好

> 注意：这个原则并不是绝对的，有时候我们需要牺牲数据的冗余度来换取数据处理的效率。

#### 数据库对象编写建议

**关于库：**
- 【强制】库的名称必须控制在32个字符以内，只能使用英文字母、数字和下划线，建议以英文字母开头。
- 【强制】库名中英文**一律小写**，不同单词采用**下划线**分割。须见名知意。
- 【强制】库的名称格式：业务系统名称_子系统名。
- 【强制】库名禁止使用关键字（如type,order等）。
- 【强制】创建数据库时必须 显式指定字符集 ，并且字符集只能是utf8或者utf8mb4。
- 创建数据库SQL举例：CREATE DATABASE crm_fund DEFAULT CHARACTER SET ‘utf8’ ;
- 【建议】对于程序连接数据库账号，遵循**权限最小原则**
使用数据库账号只能在一个DB下使用，不准跨库。程序使用的账号**原则上不准有drop权限**。
- 【建议】临时库以``tmp_``为前缀，并以日期为后缀；备份库以``bak_``为前缀，并以日期为后缀。

**关于表，列：**
- 【强制】表和列的名称必须控制在32个字符以内，表名只能使用英文字母、数字和下划线，建议以英文字母开头 。
- 【强制】 表名、列名一律小写 ，不同单词采用下划线分割。须见名知意。
- 【强制】表名要求有模块名强相关，同一模块的表名尽量使用 统一前缀 。比如：crm_fund_item
- 【强制】创建表时必须 显式指定字符集 为utf8或utf8mb4。
- 【强制】表名、列名禁止使用关键字（如type,order等）。
- 【强制】创建表时必须 显式指定表存储引擎 类型。如无特殊需求，一律为InnoDB。
- 【强制】建表必须有comment。
- 【强制】字段命名应尽可能使用表达实际含义的英文单词或 缩写 。如：公司 ID，不要使用corporation_id, 而用corp_id 即可。
- 【强制】布尔值类型的字段命名为 is_描述 。如member表上表示是否为enabled的会员的字段命名为 is_enabled。
- 【强制】禁止在数据库中存储图片、文件等大的二进制数据通常文件很大，短时间内造成数据量快速增长，数据库进行数据库读取时，通常会进行大量的随机IO操作，文件很大时，IO操作很耗时。通常存储于文件服务器，数据库只存储文件地址信息。
- 【建议】建表时关于主键： 表必须有主键 (1)强制要求主键为id，类型为int或bigint，且为auto_increment 建议使用unsigned无符号型。 (2)标识表里每一行主体的字段不要设为主键，建议设为其他字段如user_id，order_id等，并建立unique key索引。因为如果设为主键且主键值为随机插入，则会导致innodb内部页分裂和大量随机I/O，性能下降。
- 【建议】核心表（如用户表）必须有行数据的 创建时间字段 （create_time）和 最后更新时间字段 （update_time），便于查问题。
- 【建议】表中所有字段尽量都是 NOT NULL 属性，业务可以根据需要定义 DEFAULT值 。 因为使用NULL值会存在每一行都会占用额外存储空间、数据迁移容易出错、聚合函数计算结果偏差等问题。
- 【建议】所有存储相同数据的 列名和列类型必须一致 （一般作为关联列，如果查询时关联列类型不一致会自动进行数据类型隐式转换，会造成列上的索引失效，导致查询效率降低）。
- 【建议】中间表（或临时表）用于保留中间结果集，名称以 tmp_ 开头。备份表用于备份或抓取源表快照，名称以 bak_ 开头。中间表和备份表定期清理。
- 【示范】一个较为规范的建表语句：
  ```sql
  CREATE TABLE user_info (
  `id` int unsigned NOT NULL AUTO_INCREMENT COMMENT '自增主键',
  `user_id` bigint(11) NOT NULL COMMENT '用户id',
  `username` varchar(45) NOT NULL COMMENT '真实姓名',
  `email` varchar(30) NOT NULL COMMENT '用户邮箱',
  `nickname` varchar(45) NOT NULL COMMENT '昵称',
  `birthday` date NOT NULL COMMENT '生日',
  `sex` tinyint(4) DEFAULT '0' COMMENT '性别',
  `short_introduce` varchar(150) DEFAULT NULL COMMENT '一句话介绍自己，最多50个汉字',
  `user_resume` varchar(300) NOT NULL COMMENT '用户提交的简历存放地址',
  `user_register_ip` int NOT NULL COMMENT '用户注册时的源ip',
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE
  CURRENT_TIMESTAMP COMMENT '修改时间',
  `user_review_status` tinyint NOT NULL COMMENT '用户资料审核状态，1为通过，2为审核中，3为未
  通过，4为还未提交审核',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uniq_user_id` (`user_id`),
  KEY `idx_username`(`username`),
  KEY `idx_create_time_status`(`create_time`,`user_review_status`)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='网站用户基本信息'
  ```

**关于索引：**
- 【强制】InnoDB表必须主键为id int/bigint auto_increment，且主键值 禁止被更新 。
- 【强制】InnoDB和MyISAM存储引擎表，索引类型必须为 BTREE 。
- 【建议】主键的名称以 pk_ 开头，唯一键以 uni_ 或 uk_ 开头，普通索引以 idx_ 开头，一律使用小写格式，以字段的名称或缩写作为后缀。
- 【建议】多单词组成的columnname，取前几个单词首字母，加末单词组成column_name。如: sample 表 member_id 上的索引：idx_sample_mid。
- 【建议】单个表上的索引个数 不能超过6个 。
- 【建议】在建立索引时，多考虑建立 联合索引 ，并把区分度最高的字段放在最前面。
- 【建议】在多表 JOIN 的SQL里，保证被驱动表的连接列上有索引，这样JOIN 执行效率最高。
- 【建议】建表或加索引时，保证表里互相不存在 冗余索引 。 比如：如果表里已经存在key(a,b)， 则key(a)为冗余索引，需要删除。

**关于SQL编写：**
- 【强制】程序端SELECT语句必须指定具体字段名称，禁止写成 *。
- 【建议】程序端insert语句指定具体字段名称，不要写成INSERT INTO t1 VALUES(…)。
- 【建议】除静态表或小表（100行以内），DML语句必须有WHERE条件，且使用索引查找。
- 【建议】INSERT INTO…VALUES(XX),(XX),(XX)… 这里XX的值不要超过5000个。 值过多虽然上线很快，但会引起主从同步延迟。
- 【建议】SELECT语句不要使用UNION，推荐使用UNION ALL，并且UNION子句个数限制在5个以内。
- 【建议】线上环境，多表 JOIN 不要超过5个表。
- 【建议】减少使用ORDER BY，和业务沟通能不排序就不排序，或将排序放到程序端去做。ORDER BY、GROUP BY、DISTINCT 这些语句较为耗费CPU，数据库的CPU资源是极其宝贵的。
- 【建议】包含了ORDER BY、GROUP BY、DISTINCT 这些查询的语句，WHERE 条件过滤出来的结果集请保持在1000行以内，否则SQL会很慢。
- 【建议】对单表的多次alter操作必须合并为一次对于超过100W行的大表进行alter table，必须经过DBA审核，并在业务低峰期执行，多个alter需整合在一起。 因为alter table会产生 表锁 ，期间阻塞对于该表的所有写入，对于业务可能会产生极大影响。
- 【建议】批量操作数据时，需要控制事务处理间隔时间，进行必要的sleep。
- 【建议】事务里包含SQL不超过5个。因为过长的事务会导致锁数据较久，MySQL内部缓存、连接消耗过多等问题。
- 【建议】事务里更新语句尽量基于主键或UNIQUE KEY，如UPDATE… WHERE id=XX; 否则会产生间隙锁，内部扩大锁定范围，导致系统性能下降，产生死锁。

#### PowerDesigner的使用
PowerDesigner是一款开发人员常用的数据库建模工具，用户利用该软件可以方便地制作**数据流程图、概念数据模型、物理数据模型**，它几乎包括了数据库模型设计的全过程，是Sybase公司为企业建模和设计提供的一套完整的集成化企业级建模解决方案。

### 数据库的调优策略
**目的**：
- 尽可能 节省系统资源 ，以便系统可以提供更大负荷的服务。（吞吐量更大）
- 合理的结构设计和参数调整，以提高用户操作响应的速度。（响应速度更快）
- 减少系统的瓶颈，提高MySQL数据库整体的性能。

**定位调优问题**：
- 用户的反馈（主要）
- 日志分析（主要）
- 服务器资源使用监控
- 数据库内部状况监控
- 对事务 、 锁等待 等进行监控

**调优的维度和步骤**：
1. 选择适合的DBMS
2. 优化表设计
    - 表结构要尽量遵循三范式的原则
    - 如果查询应用比较多，尤其是需要进行多表联查的时候，可以采用**反范式**进行优化
    - **表字段的数据类型选择**，关系到了查询效率的高低以及存储空间的大小
3. 优化逻辑查询：通过改变SQL语句的内容让SQL执行效率更高效，采用的方式是对SQL语句进行等价变换，对查询进行重写。
  - SQL的查询重写包括了子查询优化、等价谓词重写、视图重写、条件简化、连接消除和嵌套连接消除等。
4. 优化物理查询：掌握的重点是对索引的创建和使用
5. 使用 Redis 或 Memcached 作为缓存
6. 库级优化
    - 读写分离
    - 数据分片：当数据量级达到千万级以上时，有时候我们需要把一个数据库切成多份，放到不同的数据库服务器上

#### 优化MySQL服务器
1. 优化服务器硬件
    - 配置较大的内存。减少磁盘I/O
    - 配置高速磁盘系统，以减少读盘的等待时间，提高响应速度
    - 合理分布磁盘I/0，把磁盘I/0分散在多个设备上，以减少资源竞争，提高并行操作能力。
    - 配置多处理器，MySQL是多线程的数据库，多处理器可同时执行多个线程。
2. 优化MySQL的参数
    - ``innodb_buffer_pool_size``
    - ``key_buffer_size``
    - ......

#### 优化数据库结构
1. 拆分表：冷热数据分离
2. 增加中间表
3. 增加冗余字段
4. 优化数据类型
    - 对整数类型数据进行优化。
    - 既可以使用文本类型也可以使用整数类型的字段，要选择使用整数类型。
    - 避免使用TEXT、BLOB数据类型
    - 避免使用ENUM类型，使用TINYINT来代替ENUM类型。
    - 使用TIMESTAMP存储时间
    - 用DECIMAL代替FLOAT和DOUBLE存储精确浮点数
5. 优化插入记录的速度
6. 使用非空约束、
7. 分析表、检查表与优化表
    - 分析表：
      ```sql
      ANALYZE [LOCAL | NO_WRITE_TO_BINLOG] TABLE tbl_name[,tbl_name]…
      ``` 
    - 检查表
      ```sql
      CHECK TABLE tbl_name [, tbl_name] ... [option] ... 
      option = {QUICK | FAST | MEDIUM | EXTENDED | CHANGED}
      ``` 
    - 优化表
      ```sql   
      OPTIMIZE [LOCAL | NO_WRITE_TO_BINLOG] TABLE tbl_name [, tbl_name] ...
      ``` 

#### 大表优化
1. 限定查询的范围
2. 读/写分离: 一主一从模式, 双主双从模式
3. 垂直拆分: 当数据量级达到 千万级 以上时，有时候我们需要把一个数据库切成多份，放到不同的数据库服务器上，减少对单一数据库服务器的访问压力。
    ![](images/index_optim.png)
    - 如果数据库中的数据表过多，可以采用垂直分库的方式，将关联的数据表部署在同一个数据库上。
    - 如果数据表中的列过多，可以采用垂直分表的方式，将一张数据表分拆成多张数据表，把经常一起使用的列放到同一张表里。
    - 优点： 可以使得列数据变小，在查询时减少读取的Block数，减少I/O次数。此外，垂直分区可以简化表的结构，易于维护。
    - 缺点： 主键会出现冗余，需要管理冗余列，并会引起 JOIN 操作。此外，垂直拆分会让事务变得更加复杂。
4. 水平拆分：水平拆分能够支持非常大的数据量存储，应用端改造也少，但分片事务难以解决，跨节点Join性能较差，逻辑复杂。
    ![](images/index_optim2.png)



## 事务
### 事务的基础知识
**事务**：**一组逻辑操作单元，使数据从一种状态变换到另一种状态**(打包的原子操作)。
**事务处理的原则**：保证所有事务都作为 一个工作单元 来执行，即使出现了故障，都不能改变这种执行方式。当在一个事务中执行多个操作时，要么所有的事务都被提交( commit )，那么这些修改就**永久**地保存下来；要么数据库管理系统将**放弃**所作的所有修改 ，整个事务回滚( rollback )到最初状态。

**事务的ACID特性**
- **原子性（atomicity）**：原子性是指事务是一个不可分割的工作单位，要么全部提交，要么全部失败回滚（没有中间状态）。
- **一致性（consistency）**：事务执行前后，数据从一个**合法性状态**变换到另外一个**合法性状态**，与现实世界逻辑保持一致。这种状态是**语义上**的而不是语法上的，跟具体的业务有关。满足**预定的约束**（世俗的约定）的状态就叫做合法的状态。
- **隔离性（isolation）**：指一个事务的执行**不能被其他事务干扰**，即一个事务内部的操作及使用的数据对**并发**的其他事务是隔离的，并发执行的各个事务之间不能互相干扰。不同事务的中间状态彼此不可见，事务执行结束才可见。
- **持久性（durability）**：一个事务一旦被提交，它对数据库中数据的改变就是**永久性的**，接下来的其他操作和数据库故障不应该对其有任何影响。
持久性是通过**事务日志**来保证的。日志包括了**重做日志**和**回滚日志**。当我们通过事务对数据进行修改的时候，首先会将数据库的变化信息记录到重做日志中，然后再对数据库中对应的行进行修改。这样做的好处是，即使数据库系统崩溃，数据库重启后也能找到没有更新到数据库系统中的重做日志，重新执行，从而使事务具有持久性。

> ACID是事务的四大特性，在这四个特性中，原子性是基础，隔离性是手段，一致性是约束条件，而持久性是我们的目的。

**事务的状态**
![](images/transaction_states.png)

- **活动的（active）**: 事务对应的数据库操作正在执行过程中时，我们就说该事务处在**活动的**状态。
- **部分提交的（partially committed）**: 当事务中的最后一个操作执行完成，但由于操作都在内存中执行，所造成的影响并**没有刷新到磁盘时**，我们就说该事务处在**部分提交的**状态。
- **提交的（committed）**: 当一个处在**部分提交的**状态的事务将修改过的数据都**同步到磁盘**上之后，我们就可以说该事务处
在了**提交的**状态。
- **失败的（failed）**: 当事务处在**活动的**或者**部分提交的**状态时，可能遇到了某些错误（数据库自身的错误、操作系统错误或者直接断电等）而无法继续执行，或者人为的停止当前事务的执行，我们就说该事务处在**失败的**状态。
- **中止的（aborted）**: 如果事务执行了一部分而变为**失败的**状态，那么就需要把已经修改的事务中的操作还原到事务执行前的状态(**回滚**)。当**回滚**操作执行完毕时，该事务处在了**中止的**状态。

#### 事务的使用
使用事务有两种方式，分别为**显式事务**和**隐式事务**。

<h5 align="left"> 显式事务 </h5>

- **步骤1**：``START TRANSACTION`` 或者 ``BEGIN``，作用是显式开启一个事务
  ```sql
  mysql> BEGIN;
  #或者
  mysql> START TRANSACTION;
  ```
  ``START TRANSACTION``语句相较于 ``BEGIN``特别之处在于，后边能跟随几个修饰符：``read only / read write (默认) / with consistent snapshot(启动一致性读)``
- **步骤2**：一系列事务中的操作（主要是DML，不含DDL）
- **步骤3**：事务结束的状态：提交的状态(COMMIT) 、 中止的状态(ROLLBACK)

保存点(savepoint)：一个事物可以有多个保存点。

```sql
#创建保存点
SAVEPOINT 保存点名称;

#删除某个保存点。
RELEASE SAVEPOINT 保存点名称

#回滚到某个保存点
ROLLBACK TO [SAVEPOINT]
```

<h5 align="left"> 隐式事务 </h5>
如果我们不显式的使用START TRANSACTION或者BEGIN语句开启一个事务，那么每一条语句都算是一个独立的事务，这种特性称之为事务的自动提交。
```sql
SHOW VARIABLES LIKE 'autocommit';#默认是ON
UPDATE account SET balance = balance - 10 WHERE id = 1; #此时这条DML操作是一个独立的事务
UPDATE account SET balance = balance + 10 WHERE id = 2; #此时这条DML操作是一个独立的事务
```

关闭自动提交：
- 显式的的使用 START TRANSACTION 或者 BEGIN 语句开启一个事务。这样在本次事务提交或者回滚前会暂时关闭掉自动提交的功能。
- ```sql
  SET autocommit = FALSE; #针对于DML操作是有效的，对DDL操作是无效的。
  ```

隐式提交数据的情况：
- **数据定义语言**（Data definition language，缩写为：DDL）:使用CREATE、ALTER、DROP等语句去修改数据库对象时，就会隐式的提交前边语句所属于的事务。
- **隐式使用或修改mysql数据库中的表**：当我们使用ALTER USER、CREATE USER、DROP USER、GRANT、RENAME USER、REVOKE、SETPASSWORD 等语句时也会隐式的提交前边语句所属于的事务。
- **事务控制或关于锁定的语句**
  - 当我们在一个事务还没提交或者回滚时就又使用 START TRANSACTION 或者 BEGIN 语句开启了另一个事务时，会 隐式的提交 上一个事务
  - 当前的 autocommit 系统变量的值为 OFF ，我们手动把它调为 ON 时，也会 隐式的提交前边语句所属的事务。
  - 使用 LOCK TABLES 、 UNLOCK TABLES 等关于锁定的语句也会 隐式的提交前边语句所属的事务。
- **加载数据的语句**：使用LOAD DATA语句来批量往数据库中导入数据时，也会隐式的提交前边语句所属的事务。
- **关于MySQL复制的一些语句**：使用START SLAVE、STOP SLAVE、RESET SLAVE、CHANGE MASTER TO等语句时会隐式的提交前边语句所属的事务。
- 其它的一些语句：使用ANALYZE TABLE、CACHE INDEX、CHECK TABLE、FLUSH、LOAD INDEX INTO CACHE、OPTIMIZE TABLE、REPAIR TABLE、RESET等语句也会隐式的提交前边语句所属的事务。

#### 事务隔离级别
每个客户端与服务器连接上之后，就可以称为一个会话（ Session ）

**事物并发问题：**
1. **脏写**（ Dirty Write ）：对于两个事务 Session A、Session B，如果事务Session A **修改了**另一个**未提交**事务Session B **修改过** 的数据，那就意味着发生了脏写
2. **脏读**（ Dirty Read ）: 对于两个事务 Session A、Session B，Session A **读取了**已经被 Session B **更新**但还**没有被提交**的字段。之后若 Session B 回滚 ，Session A 读取 的内容就是**临时且无效**的。
3. **不可重复读**（ Non-Repeatable Read ）：对于两个事务Session A、Session B，Session A **读取**了一个字段，然后 Session B **更新**了该字段。 之后Session A 再次读取 同一个字段， 值就不同了。

4. **幻读**（ Phantom ）：对于两个事务Session A、Session B, Session A 从一个表中**读取**了一个字段, 然后 Session B 在该表中**插入**了一些新的行。 之后, 如果 Session A 再次读取 同一个表, 就会多出几行。

**SQL中的四种隔离级别：**
事物并发严重程度排序：
```
脏写 > 脏读 > 不可重复读 > 幻读
```
![](images/transaction_isolation.png)
**脏写比较严重，每种隔离级别都不允许脏写**
1. ``READ UNCOMMITTED`` ：读未提交，在该隔离级别，所有事务都可以看到其他未提交事务的执行结果。不能避免脏读、不可重复读、幻读。
2. ``READ COMMITTED``：读已提交，它满足了隔离的简单定义：一个事务只能看见已经提交事务所做的改变。这是大多数数据库系统的默认隔离级别（但不是MySQL默认的）。可以避免脏读，但不可重复读、幻读问题仍然存在。
3. ``REPEATABLE READ``：可重复读，事务A在读到一条数据之后，此时事务B对该数据进行了修改并提交，那么事务A再读该数据，读到的还是原来的内容。可以避免脏读、不可重复读，但幻读问题仍然存在。这是MySQL的默认隔离级别。
4. ``SERIALIZABLE``：可串行化，确保事务可以从一个表中读取相同的行。在这个事务持续期间，禁止其他事务对该表执行插入、更新和删除操作。所有的并发问题都可以避免，但性能十分低下。能避免脏读、不可重复读和幻读。

种事务隔离级别与并发性能的关系如下：
<div align=center>
   <img src="images/transaction_isolation2.png" width = "70%" height = "70%">
</div>

**MySQL支持的四种隔离级别**
```sql
mysql> SHOW VARIABLES LIKE 'transaction_isolation';
+-----------------------+-----------------+
| Variable_name         | Value           |
+-----------------------+-----------------+
| transaction_isolation | REPEATABLE-READ |
+-----------------------+-----------------+
1 row in set (0.00 sec)
```

设置隔离级别：
```sql
SET [GLOBAL|SESSION] TRANSACTION_ISOLATION = '隔离级别'
#其中，隔离级别格式：
> READ-UNCOMMITTED
> READ-COMMITTED
> REPEATABLE-READ
> SERIALIZABLE
```

#### 事务的常见分类
1. 扁平事务（Flat Transactions）: 由BEGIN WORK开始，由COMMIT WORK或ROLLBACK WORK结束
2. 带有保存点的扁平事务（Flat Transactions with Savepoints）
3. 链事务（Chained Transactions）:  在提交一个事务时，释放不需要的数据对象，将必要的处理上下文隐式地传给下一个要开始的事务，前一个子事务的提交操作和下一个子事务的开始操作合并成一个原子操作。
4. 嵌套事务（Nested Transactions）：是一个层次结构框架，由一个顶层事务(Top-Level Transaction）控制着各个层次的事务
5. 分布式事务（Distributed Transactions）：通常是在一个分布式环境下运行的扁平事务，跨行转账。

事务有4种特性：原子性、一致性、隔离性和持久性。
- 事务的隔离性由**锁机制**实现。
- 而事务的原子性、一致性和持久性由事务的 redo 日志和undo 日志来保证。redo 日志和undo 日志都是由存储引擎生成
  - REDO LOG 称为**重做日志**，提供再写入操作，恢复提交事务修改的页操作，用来保证事务的持久性。记录**物理级别**页的修改，如页号和偏移量。
  - UNDO LOG 称为**回滚日志**，回滚行记录到某个特定版本，用来保证事务的原子性、一致性。记录**逻辑操作**日志，如INSERT。主要用于**事务回滚**和**一致性非锁定读**（多版本并发控制）。

### REDO LOG

为了**保证在内存中已commit的数据能够写入到硬盘**中（哪怕突然断电也能恢复），使用redo日志。

一个简单的做法 ：在事务提交完成之前把该事务所修改的所有页面都刷新到磁盘，但是这个简单粗暴的做法有些问题
- 修改量与刷新磁盘工作量严重不成比例
- 随机I/O较慢

**好处：**
- redo日志降低了刷盘频率
- redo日志占用的空间非常小
  
**特点：**
- **redo日志是顺序写入磁盘的**:在执行事务的过程中，每执行一条语句，就可能产生若干条redo日志，这些日志是按照产生的顺序写入磁盘的， 也就是使用顺序I/O，效率比随机I/O快。
- **事务执行过程中，redo log不断记录**: redo log 跟bin log 的区别， redo log是**存储引擎层**产生的，而bin log是**数据库层**产生的。假设一个事务，对表做 10万行的记录揷入，在这个过程中，一直不断的是redo log顺序记录，而bin log不会记录，直到这个事务提交，オ 会一次写入到bin log文件中。

**组成：**
- **重做日志的缓冲 (redo log buffer)**，保存在连续内存中，是易失的。
  - 参数设置：innodb_log_buffer_size：redo log buffer 大小，默认**16M**，最大值是4096M，最小值为1M。
    ```sql
    mysql> show variables like '%innodb_log_buffer_size%';
    +------------------------+----------+
    | Variable_name | Value |
    +------------------------+----------+
    | innodb_log_buffer_size | 16777216 |
    +------------------------+----------+
    ```
    redo log buffer被划分为若干连续 **redo log block(512字节大小)**。
    ![](images/transaction_redo_log_buffer.png)

- **重做日志文件 (redo log file)**，保存在硬盘中，是持久的(从这里恢复)。

**redo的整体流程：**
![](images/log_redo_structure.png)

**redo log的刷盘:**
InnoDB引擎会在写redo log的时候先写redo log buffer，然后redo log buffer被写入到**文件系统缓存**（page cache）中，**文件系统缓存**由操作系统管理，将redo log buffer刷盘到 redo log file中。
InnoDB给出``innodb_flush_log_at_trx_commit``参数，该参数控制 commit 提交事务时，如何将 redo log buffer 中的日志刷新到 redo log file 中。它支持三种策略：
- 设置为0 ：表示每次事务提交时不进行刷盘操作。（系统默认master thread每隔1s进行一次重做日志的同步）
- 设置为1 ：表示每次事务提交时都将进行同步，刷盘操作（ 默认值 ）
- 设置为2 ：表示每次事务提交时都只把 redo log buffer 内容写入 page cache，不进行同步。

![](images/log_redo_0.png)
![](images/log_redo_1.png)
![](images/log_redo_2.png)

**写入redo log buffer 过程：**
MySQL把对底层页面中的一次原子访问的过程称之为一个Mini-Transaction，简称 mtr。一个所谓的 mtr 可以包含一组redo日志。
![](images/log_redo_mtr.png)
![](images/log_redo_log_block.png)
![](images/log_redo_log_buffer.png)

**redo log file:**

1. 相关参数设置
- ``innodb_log_group_home_dir`` ：指定 redo log 文件组所在的路径，默认值为 ./ ，表示在数据库的数据目录下。MySQL的默认数据目录（var/lib/mysql）下默认有两个名为 ib_logfile0 和 ib_logfile1 的文件
- ``innodb_log_files_in_group``：指明redo log file的个数，默认2个，最大100个。
  ```sql
  mysql> show variables like 'innodb_log_files_in_group';
  ```
- ``innodb_flush_log_at_trx_commit``：控制 redo log 刷新到磁盘的策略，默认为1。
- ``innodb_log_file_size``：单个redo log文件设置大小，默认值为 **48M**。最大值为512G。
  ```sql
  mysql> show variables like 'innodb_log_file_size';
  ```
2. 日志文件组
![](images/log_redo_group.png)
总共的redo日志文件大小其实就是：``innodb_log_file_size×nnodb_log_files_in_group``。
采用循环使用的方式向redo日志文件组里写数据的，防止后面写入的覆盖前面的，InnoDB的设计者提出了checkpoint的概念。
3. checkpoint
- ``write pos`` 是当前记录的位置，一边写一边后移
- ``checkpoint`` 是当前要擦除的位置，也是往后推移
- 如果 write pos 追上 checkpoint ，表示**日志文件组**满了，这时候不能再写入新的 redo log记录，MySQL 得停下来，清空一些记录，把checkpoint推进一下。

### UNDO LOG 
redo log是事务持久性的保证，undo log是事务原子性的保证。在事务中**更新数据**的**前置操作**其实是要先写入一个 undo log。Write Ahead Log(先写日志，再写磁盘)。在InnoDB存储引擎中，undo log分为：``insert undo log``和``update undo log``。
> undo log也会差生redo log

**作用：**
- **回滚数据**： 逻辑回滚而非物理回滚
- **MVCC**: InnoDB存储引擎中MVCC的实现是通过undo来完成。当用户读取一行记最时，若该记录已经被其他事务占用，当前事务可以通过undo读取之前的行版本信息，以此实现非锁定读取。

**undo的存储结构:**
1. 回滚段与undo页
InnoDB对undo log的管理采用**回滚段**（rollback segment）方式。每个回滚段记录了1024 个``undo log segment``，而在每个undo log segment段中进行**undo页**的申请。InnoDB目前支持最大128个rollback segment。
    ```sql
    mysql> show variables like 'innodb_undo_logs';
    +------------------+-------+
    | Variable_name | Value |
    +------------------+-------+
    | innodb_undo_logs | 128 |
    +------------------+-------+
    ```
2. 回滚段与事务
- 每个事务只会使用一个回滚段，一个回滚段在同一时刻可能会服务于多个事务
- 当一个事务开始的时候，会制定一个回滚段，在事务进行的过程中，当数据被修改时，原始的数据会被复制到回滚段。
- 在回滚段中，事务会不断填充盘区，直到事务结束或所有的空间被用完。如果当前的盘区不够用，事务会在段中请求扩展下一个盘区，如果所有已分配的盘区都被用完，事务会覆盖最初的盘区或者在回滚段允许的情况下扩展新的盘区来使用。
- 回滚段存在于undo表空间中，在数据库中可以存在多个undo表空间，但同一时刻只能使用一个undo表空间。
- 当事务提交时，InnoDB存储引擎会做以下两件事情：
  - 将undo log放入列表中，以供之后的purge操作
  - 判断undo log所在的页是否可以重用，若可以分配给下个事务使用
3. 回滚段中的数据分类
- 未提交的回滚数据(uncommitted undo information)
- 已经提交但未过期的回滚数据(committed undo information)
- 事务已经提交并过期的数据(expired undo information)

**undo log的生命周期：**
生成：
![](images/log_undo_log.png)
undo log的删除:
- 针对于insert undo log：undo log可以在事务提交后直接删除，不需要进行purge操作。
- 针对于update undo log：该undo log可能需要提供MVCC机制，因此不能在事务提交时就进行删除。提交时放入undo log链表，等待purge线程进行最后的删除。

### 锁
事务的**隔离性**由**锁**来实现。
并发事物访问同一记录分三种情况：
1. 读-读：可以
2. 写-写：任何一种隔离级别都不允许**脏写**的发生。每个事物对应一把锁，每一时刻的记录只能获取一个事物的锁，保证事物互斥访问对应记录。
3. 读-写：可能发生**脏读、不可重复读、幻读**的问题。解决方案
    - 读操作利用**多版本并发控制**（MVCC），写操作进行**加锁**。 **读-写**操作彼此并不冲突，**性能更高**。
      > 普通的SELECT语句在READ COMMITTED和REPEATABLE READ隔离级别下会使用到MVCC读取记录。
    - 读、写操作都采用**加锁**的方式。**读-写**操作彼此需要**排队执行**，影响性能。

#### 从数据的操作类型划分：共享锁、排他锁
**共享锁**: S 锁。针对同一份数据，多个事务的读操作可以同时进行而不会互相影响，相互不阻塞的。
**排他锁**: X 锁。当前写操作没有完成前，它会阻断其他写锁和读锁。
> 需要注意的是对于 InnoDB 引擎来说，读锁和写锁可以加在表上，也可以加在行上。读操作(SELECT)可以加X锁或S锁, 写操作(DELETE, INSERT, UPDATE)必须加X锁。 事物 commit 后锁才释放。 INSERT一般隐式锁保护。

| |  X锁  |  S锁  |  
|:----| :---: | :---:   |
|X锁 |兼容     | 兼容   |
|S锁 | 兼容    |不兼容  | 

读操作加锁：
```sql
SELECT ...... LOCK IN SHARE MODE; # 对读取的记录加S锁
SELECT ...... FOR SHARE; # 对读取的记录加S锁

SELECT ...... FOR UPDATE; # 对读取的记录加X锁
```

#### 从数据操作的粒度划分：表级锁、页级锁、行级锁
<h4 align="left">表级锁(Table Lock)</h4>

**表级锁**不依赖于存储引擎，表锁**开销最小**，可以**避免死锁**，**并发性差**。 InnoDB一般不使用表级锁。

1. **表级别的读锁、写锁**
一般情况下，不会使用InnoDB存储引擎提供的表级别的 **读锁** 和 **写锁**。只会在一些特殊情况下使用，比方说**崩溃恢复**过程。

在系统变量 ``autocommit=0，innodb_table_locks = 1`` 时， 手动获取
InnoDB存储引擎提供的表t 的 **读锁** 或者**写锁**：
- ``LOCK TABLES t READ`` ：InnoDB存储引擎会对表 t 加表级别的读锁 。
- ``LOCK TABLES t WRITE`` ：InnoDB存储引擎会对表 t 加表级别的写锁 。

| |自己可读|自己可写|自己可操作其他表 |其他人可读| 他人可写|
|:----:| :---: | :---:  |    :---:      | :---:   | :---:   |
|表级读锁 |是  | 否     |   否           |是      |否，等    |
|表级写锁 |是  |是      |   否           |否，等   |否，等   |

2. **意向锁(intention lock)**
  
**意向锁**表锁一种，它允许**行级锁**与**表级锁**共存。表的意向锁**不与该表的行级锁冲突**。
**目的**：解决表锁与之前可能存在的行锁冲突，避免为了判断表是否存在行锁而去扫描全表的系统消耗。
**来源**：意向锁是由存储引擎自己维护的，如果表中某一行记录被加上S/X锁，数据库自动给更高一级的空间加上意向锁。
**意向锁的加锁规则**：
- 事务在获取行级 S 锁之前，必须获取其对应表的 IS 或 IX 锁
- 事务在获取行级 X 锁之前，必须获取其对应表的 IX 锁。
  
**例**：事务A给 Tab 的某一行记录 item1 加 X 锁，事务B给 Tab 的某一行记录 item2 加 X 锁，不冲突(表级的IX和IX兼容， 两条记录的行锁不冲突)。

同一级别锁比较（不是表级别锁和行级别锁比较）：

|     |  IS  |  IX   |  S   | X      |
|:----| :---:| :---: | :---:| :---:  |
|IS  |兼容   | 兼容   | 兼容  | 不兼容 |
|IX  | 兼容  |兼容    | 不兼容|不兼容  |
|S   |兼容   | 不兼容 | 兼容  | 不兼容 |
|X   |不兼容 | 不兼容 | 不兼容| 不兼容 |

3. **自增锁（AUTO-INC锁）**

自增锁是一种表级锁（table-level lock），**专门针对插入AUTO_INCREMENT类型的列**。同一表，假设事务 A 正在插入数据，则另一个事务 B 尝试 INSERT 语句，事务 B 会被阻塞住，直到事务 A 释放自增锁，以便事务A插入的行是连续的主键 ID

插入方式：
| 插入方式|  解释  |   
|:----| :--- | 
|**Simple Inserts（简单插入）** |可以预估插入行数的语句（普通的 insert/replace into语句）；但不包含像 insert … on duplicate key update … 插入或者更新的语句     | 
|**Bulk Inserts（批量插入）** | 无法预估插入行数的语句（包括 insert ... select, replace ... select 和 load data 语句 ） |
|**Mixed-mode insert (混合插入)** | 类似 insert into t1(id, age) values (1,"zhang3"),(null, "li4"),(5,"wang5");有些行指定了自增id，有些行未指定自增id |

自增锁模式：
| 模式|  解释  |  innodb_autoinc_lock_mode  |  
|:----| :--- | :---:   |
|传统模式 |执行语句时加 AUTO-INC 表级锁，statement 语句执行完毕后释放     | 0   |
|连续模式 | 针对批量插入 时会采用 AUTO-INC 锁，针对简单插入时，采用轻量级的互斥锁    |1  | 
|混合模式 | 不使用 AUTO-INC 表级锁 ，采用轻量级的互斥锁    |2 | 

4. **元数据锁**

元数据锁(metadata lock, MDL)是server层的锁，表级锁，每执行一条DML、DDL语句时都会申请MDL锁，**DML操作（增删改）需要MDL读锁，DDL操作（变更表结构）需要MDL写锁**（MDL加锁过程是系统自动控制，无法直接干预，读读共享，读写互斥，写写互斥），申请MDL锁的操作会形成一个队列，队列中写锁获取优先级高于读锁。一旦出现写锁等待，不但当前操作会被阻塞，同时还会阻塞后续该表的所有操作。

<h4 align="left">行锁</h4>


行锁(Row Lock)也称为记录锁，顾名思义，就是锁住某一行（某条记录row)。行级锁只在存储引擎层实现。

- **优点**:锁定力度小，发生**锁冲突概率低**，可以实现的**并发度高**。
- **缺点**:对于锁的**开销比较大**，加锁会比较慢，容易出现**死锁**情况。
  
> InnoDB与MylSAM的最大不同有两点:一是支持事务（TRANSACTION);二是采用了行级锁。

1. **记录锁（Record Locks）**

- 隐式加锁：
  - InnoDB自动加意向锁。
  - 对于UPDATE、DELETE和INSERT语句，InnoDB会自动给涉及数据集加排他锁（X)；
  - 对于普通SELECT语句，InnoDB不会加任何锁；
- 显示加锁：
    ```sql
    SELECT ...... LOCK IN SHARE MODE; # 对读取的记录加S锁
    SELECT ...... FOR SHARE; # 对读取的记录加S锁

    SELECT ...... FOR UPDATE; # 对读取的记录加X锁
    ```
2. **间隙锁（Gap Locks）**

gap锁的提出是为了防止插入幻影记录而提出的，防止幻读；以及为了数据恢复和复制的需要。
![](images/lock_gap_locks.png)

加锁方式：
```sql
select * from student where id = 5 lock in share mode;
```
id=5在(3, 8)之间, 即**不允许在id列的值(3, 8)这个区间的插入新记录**，这个区间被加上了间隙锁。id是5还是6都一样。
```sql
select * from student where id = 21 lock in share mode;
```
id列的值(20, +$\infty$)这个区间加了间隙锁
> X间隙锁和S间隙锁没区别(都是保护对应区间，不让在对应区间内插入记录)，不同事务对同一个区间可以重复加间隙锁。

容易死锁：
```sql
# 事物A
BEGIN;
SELECT * FROM student WHERE id = 5 LOCK IN SHARE MODE;

# 事物B
BEGIN;
INSERT INTO student(id, name, class) VALUES(6, 'Jerry', '一班');
# 这里容易出现死锁，可以设定时间，超时自动结束
```

3. **临键锁（Next-Key Locks）**

有时候我们既想**锁住某条记录**，又想**阻止**其他事务在该记录前边的**间隙插入新记录**(兼具记录锁和间隙锁)，所以InnoDB就提出**临键锁（Next-Key Locks）**。之前不允许在id列的值(3, 8)这个区间插入记录，现在不允许在(3, 8]这个区间插入记录。
```sql
begin;
select * from student where id <=8 and id > 3 for update;
```

4. **插入意向锁（Insert Intention Locks）**

事物A获取某个区间的间隙锁，此时事物B往间隙插入数据需要等待，**InnoDB规定事务在等待的时候也需要在内存中生成一个锁结构**，表明有事务想在某个**间隙**中**插入**新记录，但现在在等待。插入意向锁是在插入一条记录行前，由 **INSERT 操作产生的一种间隙锁**。**插入意向锁并不会阻止别的事务继续获取该记录上任何类型的锁**。

**参考：**
- [mysql锁机制详解](https://www.cnblogs.com/volcano-liu/p/9890832.html)
- [MySQL中的锁（表锁、行锁）](https://www.cnblogs.com/chenqionghe/p/4845693.html)
- [MySQL锁详解](https://www.cnblogs.com/luyucheng/p/6297752.html)
- [意向锁到底是什么](https://blog.csdn.net/a1102325298/article/details/86586629)
- [MySQL 之 innodb 自增锁原理实现](https://juejin.cn/post/7106271665281040421)


<h4 align="left">页锁</h4>

页锁的开销介于表锁和行锁之间，会出现死锁。锁定粒度介于表锁和行锁之间，并发度一般。

#### 从对待锁的态度划分:乐观锁、悲观锁

从对待锁的态度来看锁的话，可以将锁分成乐观锁和悲观锁。
- **悲观锁（Pessimistic Locking）**: 悲观锁总是假设最坏的情况，对数据被其他事务的修改持保守态度，**通过数据库自身的锁机制来实现**。每次在拿数据的时候都会上锁，即**共享资源每次只给一个线程使用，其它线程阻塞，用完后再把资源转让给其它线程**。并发性差，安全性高。
- **乐观锁（Optimistic Locking）**: 乐观锁认为对同一数据的并发操作不会总发生，属于小概率事件，**不采用数据库自身的锁机制，而是通过程序来实现**。在程序上，我们可以采用**版本号机制**或者**CAS机制**实现。
> 乐观锁适合**读操作多**的场景，悲观锁适合**写操作多**的场景

#### 按加锁的方式划分：显式锁、隐式锁
- **隐式锁**： 
  - INSERT一条记录
  - 插入意向锁
- **显示锁**：显式的添加记录

#### 其它锁: 全局锁、死锁
1. **全局锁**: 就是对**整个数据库实例**加锁。当你需要让整个库处于**只读状态**的时候。全局锁的典型使用场景是：**做全库逻辑备份**。
  ```sql
  Flush tables with read lock
  ```
2. **死锁**

死锁是指两个或多个事务在同一资源上相互占用，并请求锁定对方占用的资源，从而导致恶性循环。

产生死锁的必要条件：
  - 两个或者两个以上事务
  - 每个事务都已经持有锁并且申请新的锁
  - 锁资源同时只能被同一个事务持有或者不兼容
  - 事务之间因为持有锁和申请锁导致彼此循环等待

例：
![](images/lock_dead_lock.png)
事务1在等待事务2释放id=2的行锁，而事务2在等待事务1释放id=1的行锁。

死锁解决方法：
- 直接进入等待，直到**超时**。这个超时时间可以通过参数innodb_lock_wait_timeout来设置。
- 发起**死锁检测**，发现死锁后，主动回滚死锁链条中的某一个事务（将持有最少行级
排他锁的事务进行回滚），让其他事务得以继续执行。将参数 innodb_deadlock_detect 设置为
on ，表示开启这个逻辑。

#### 锁的内存结构
如果一个事务要获取 10000 条记录的锁，生成 10000 个锁结构消耗太大! 如果符合下边这些条件的记录会放到一个锁结构中。
- 在同一个事务中进行加锁操作
- 被加锁的记录在同一个页面中
- 加锁的类型是一样的
- 等待状态是一样的

![](images/lock_structure.png)
1. 锁所在的事务信息: 指针
2. 索引信息：指针
3. 表锁／行锁信息
  - 表锁：记载着是对哪个表加的锁，还有其他的一些信息。
  - 行锁：记载了三个重要的信息：
    - Space ID ：记录所在表空间。
    - Page Number ：记录所在页号。
    - n_bits ：表征页内哪些记录加了行锁
4. type_mode ：这是一个32位的数，被分成了 lock_mode 、 lock_type 和 rec_lock_type 三个部分
5. 其他信息：为了更好的管理系统运行过程中生成的各种锁结构而设计了各种哈希表和链表。
6. 一堆比特位：对应着一个页面中的记录，一个比特位映射一个 heap_no ，即一个比特位映射到页内的一条记录

#### 锁监控
通过检查 ``InnoDB_row_lock`` 等状态变量来分析系统上的行锁的争夺情况
```sql
mysql> show status like 'innodb_row_lock%';
+-------------------------------+-------+
| Variable_name                 | Value |
+-------------------------------+-------+
| Innodb_row_lock_current_waits | 0     |
| Innodb_row_lock_time          | 59082 |
| Innodb_row_lock_time_avg      | 29541 |
| Innodb_row_lock_time_max      | 51032 |
| Innodb_row_lock_waits         | 2     |
+-------------------------------+-------+
5 rows in set (0.00 sec)
```
- nnodb_row_lock_current_waits：当前正在等待锁定的数量；
- Innodb_row_lock_time ：从系统启动到现在锁定总时间长度；（等待总时长）
- Innodb_row_lock_time_avg ：每次等待所花平均时间；（等待平均时长）
- Innodb_row_lock_time_max：从系统启动到现在等待最常的一次所花的时间；
- Innodb_row_lock_waits ：系统启动后到现在总共等待的次数；（等待总次数）

### 多版本并发控制
MVCC在MySQL InnoDB中的实现主要是为了提高数据库**并发性能**，用更好的方式去处理**读-写冲突**，做到即使有读写冲突时，也能做到**不加锁**， 非阻塞并发读，而这个读指的就**快照读**, 而非**当前读**。当前读实际上是一种加锁的操作（加锁写操作），是悲观锁的实现。而MVCC本质是采用乐观锁思想的一种方式。
- **快照读**：快照读又叫一致性读，读取的是快照数据。不加锁的简单的 SELECT 都属于快照读
    ```sql
    SELECT * FROM player WHERE ...
    ```
- **当前读**: 当前读读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。
    ```sql
    SELECT * FROM student LOCK IN SHARE MODE; # 共享锁 
    SELECT * FROM student FOR UPDATE; # 排他锁 
    INSERT INTO student values ... # 排他锁 
    DELETE FROM student WHERE ... # 排他锁 
    UPDATE student SET ... # 排他锁
    ```
在MySQL中，默认的隔离级别是**可重复读**，可以解决**脏读**和**不可重复读**的问题，不能解决**幻读**(需要串行化隔离级别)，但MVCC可以在**可重复读**隔离级别下解决幻读问题。MVCC在``READ COMMITTED`` 和 ``REPEATABLE READ``隔离级别起作用（其他两种隔离级别相当于当前读，不需要快照读）。

#### MVCC实现原理
MVCC 的实现依赖于：``隐藏字段、Undo Log、Read View``。

**隐藏字段**: row_id, trx_id, roll_id
- trx_id ：每次一个事务对某条聚簇索引记录进行改动时，都会把该事务的**事务id**赋值给trx_id 隐藏列。
- roll_pointer ：每次对某条聚簇索引记录进行改动时，都会把旧的版本写入到**undo日志**中，然后这个隐藏列就相当于一个指针，可以通过它来找到该记录修改前的信息。

![](images/log_mvcc.png)
对该记录每次更新后，都会将旧值放到一条**undo日志**中，所有的版本都会被 ``roll_pointer`` 属性连接成一个链表，我们把这个链表称之为**版本链**，版本链的头节点就是当前记录最新的值。

**Read View:**
使用``READ COMMITTED`` 和 ``REPEATABLE READ`` 隔离级别的事务需要使用ReadView，主要**判断版本链中的哪个版本是当前事务可见的**。
ReadView的4个内容：
- creator_trx_id ，创建这个 Read View 的事务 ID。
  >说明：只有在对表中的记录做改动时（执行INSERT、DELETE、UPDATE这些语句时）才会为事务分配事务id，否则在一个只读事务中的事务id值都默认为0。
- trx_ids ，表示在生成ReadView时当前系统中活跃的读写事务的 **事务id列表**。
- up_limit_id ，活跃的事务中最小的事务 ID。
- low_limit_id ，表示生成ReadView时系统中应该分配给下一个事务的 id 值。low_limit_id 是系统最大的事务id值，这里要注意是系统中的事务id，需要区别于正在活跃的事务ID。

举例: trx_ids为trx2、trx3、trx5和trx8的集合，系统的最大事务ID(low_limit_id)为trx8+1(如果之前没有其他的新增事务)，活跃的最小事务ID (up_limit_id)为trx2。
![](images/log_mvcc2.png)

ReadView的规则：
1. 如果被访问版本的trx_id属性值与ReadView中的 ``creator_trx_id``值相同，意味着当前事务在访问它自己修改过的记录，所以该版本可以被当前事务访问。
2. 如果被访问版本的trx_id属性值小于ReadView中的 ``up_limit_id``值，表明生成该版本的事务在当前事务生成ReadView前已经提交，所以该版本可以被当前事务访问。
3. 如果被访问版本的trx_id属性值大于或等于ReadView中的``low_limit_id``值，表明生成该版本的事务在当前事务生成ReadView后才开启，所以该版本不可以被当前事务访问。
4. 如果被访问版本的trx_id属性值在ReadView的``up_limit_id``和 ``low_limit_id``之间，那就需要判断一下trx_id属性值是不是在``trx_ids列表``中。
    - 如果在，说明创建ReadView时生成该版本的事务还是活跃的，该版本不可以被访问。
    - 如果不在，说明创建ReadView时生成该版本的事务已经被提交，该版本可以被访问。

**MVCC整体操作流程**:
1. 首先获取事务自己的版本号，也就是事务 ID；
2. 获取 ReadView；
3. 查询得到的数据，然后与 ReadView 中的事务版本号进行比较；
4. 如果不符合 ReadView 规则，就需要从 Undo Log 中获取历史快照；
5. 最后返回符合规则的数据。
> - 在隔离级别为读已提交（Read Committed）时，**一个事务中的每一次 SELECT 查询都会重新获取一次Read View**。
> - 当隔离级别为可重复读的时候，就避免了不可重复读，这是因为一个事务**只在第一次 SELECT 的时候会获取一次 Read View**，而后面所有的 SELECT 都会复用这个 Read View，






## 日志与备份

### 其他数据库日志

NySQL6类日志分别为：
1. **慢查询日志**：记录所有执行时间超过long_query_time的所有查询，方便我们对查询进行优化。
2. **通用查询日志**：记录所有连接的起始时间和终止时间，以及连接发送给数据库服务器的所有指令，对我们复原操作的实际场景、发现问题，甚至是对数据库操作的审计都有很大的帮助。
3. **错误日志**：记录MySQL服务的启动、运行或停止MySQL服务时出现的问题，方便我们了解服务器的状态，从而对服务器进行维护。
4. **二进制日志**：记录所有更改数据的语句，可以用于主从服务器之间的数据同步，以及服务器遇到故障时数据的无损失恢复。
5. **中继日志**：用于主从服务器架构中，从服务器用来存放主服务器二进制日志内容的一个中间文件。从服务器通过读取中继日志的内容，来同步主服务器上的操作。
6. **数据定义语句日志**：记录数据定义语句执行的元数据操作。

日志的弊端:
1. 日志功能会**降低MySQL数据库的性能**。
2. 日志会**占用大量的磁盘空间**。
   
<h4 align="left">通用查询日志(general query log)</h4>
查看当前状态: 可以找到日志路径，直接打开。
```sql
mysql> SHOW VARIABLES LIKE '%general%';
+------------------+---------------------------+
| Variable_name    | Value                     |
+------------------+---------------------------+
| general_log      | OFF                       |
| general_log_file | /var/lib/mysql/ubuntu.log |
+------------------+---------------------------+
2 rows in set (0.00 sec)
```

启动 or 停止日志:
- 永久性方式:
    ```sql
    [mysqld]
    general_log=ON # OFF
    general_log_file=[path[filename]] #日志文件所在目录路径，filename为日志文件名
    ```
- 临时性方式:
    ```sql
    SET GLOBAL general_log=on; # 开启通用查询日志 # off
    SET GLOBAL general_log_file=’path/filename’; # 设置日志文件保存位置
    ```
删除\刷新日志：
- 手动删除：找到对应位置删除即可
- 如下命令重新生成查询日志文件（前提一定要开启通用日志）
  ```sql
  mysqladmin -uroot -p flush-logs
  ```

<h4 align="left">错误日志(error log)</h4>

启动日志: 在MySQL数据库中，错误日志功能是**默认开启**的。而且，错误日志**无法被禁止**
```sql
[mysqld]
log-error=[path/[filename]] #path为日志文件所在的目录路径，filename为日志文件名
```
查看日志:
```sql
mysql> SHOW VARIABLES LIKE 'log_err%';
+---------------------+--------------------------+
| Variable_name       | Value                    |
+---------------------+--------------------------+
| log_error           | /var/log/mysql/error.log |
| log_error_verbosity | 3                        |
+---------------------+--------------------------+
2 rows in set (0.00 sec)
```
删除\刷新日志：

- 手动删除：找到对应位置删除即可
- 如下命令重新生成查询日志文件（先备份）
  ```sql
  install -omysql -gmysql -m0644 /dev/null /var/log/mysqld.log
  mysqladmin -uroot -p flush-logs
  ```

<h4 align="left">二进制日志(bin log)</h4>

bin log也叫作变更日志（update log）。它记录了数据库所有执行的DDL 和 DML 等数据库**更新事件**(增删改)的语句，但是不包含没有修改任何数据的语句（如数据查询语句select、show等）。
binlog主要应用场景：
- 一是用于**数据恢复**
- 二是用于**数据复制**

**查看当前状态:**
```sql
mysql> show variables like '%log_bin%';
+---------------------------------+-------+
| Variable_name                   | Value |
+---------------------------------+-------+
| log_bin                         | OFF   |
| log_bin_basename                |       |
| log_bin_index                   |       |
| log_bin_trust_function_creators | OFF   |
| log_bin_use_v1_row_events       | OFF   |
| sql_log_bin                     | ON    |
+---------------------------------+-------+
6 rows in set (0.00 sec)
```

**日志参数设置:**
- 永久性方式:
  ```sql
  [mysqld]
  #启用二进制日志
  log-bin=atguigu-bin
  binlog_expire_logs_seconds=600
  max_binlog_size=100M

  # 改变日志文件的目录和名称
  log-bin="/var/lib/mysql/binlog/atguigu-bin"
  chown -R -v mysql:mysql binlog
  ```
- 临时性方式: mysql8中只有 会话级别 的设置，没有了global级别的设置。
  ```sql
  # session级别
  mysql> SET sql_log_bin=0;
  Query OK, 0 rows affected (0.01 秒)
  ```

**查看日志:**
- 查看当前的二进制日志文件列表及大小。指令如下：
  ```sql
  mysql> SHOW BINARY LOGS;
  +--------------------+-----------+-----------+
  | Log_name | File_size | Encrypted |
  +--------------------+-----------+-----------+
  | atguigu-bin.000001 | 156 | No |
  +--------------------+-----------+-----------+
  1 行于数据集 (0.02 秒)
  ```
- 将行事件以 伪SQL的形式 表现出来
  ```sql
  mysql> mysqlbinlog -v "/var/lib/mysql/binlog/atguigu-bin.000002"
  ```

使用日志恢复数据: mysqlbinlog恢复数据的语法如下
```sql
mysqlbinlog [option] filename|mysql –uuser -ppass;
```
这个命令可以这样理解：使用mysqlbinlog命令来读取filename中的内容，然后使用mysql命令将这些内容恢复到数据库中
- filename ：是日志文件名。
- option ：可选项，比较重要的两对option参数
  - start-date 和 --stop-date ：可以指定恢复数据库的起始时间点和结束时间点。
  - start-position和–stop-position ：可以指定恢复数据的开始位置和结束位置。
>编号小的先恢复

**删除二进制日志:**
- ``PURGE MASTER LOGS``:只删除指定部分的二进制日志文件
  ```sql
  PURGE {MASTER | BINARY} LOGS TO ‘指定日志文件名’ 
  PURGE {MASTER | BINARY} LOGS BEFORE ‘指定日期’
  ```
- ``RESET MASTER``:删除所有二进制日志文件
  
**二进制日志(binlog)写入**：
事务执行过程中，先把日志写到``binlog cache``，事务提交的时候，再
把binlog cache写到binlog文件中。因为一个事务的binlog不能被拆开，无论这个事务多大，也要确保一次性写入，所以系统会给每个线程分配一个块内存作为binlog cache。
通过binlog_cache_size参数控制单个线程 binlog cache大小
![](images/log_bin_log.png)
write和fsync的时机，可以由参数``sync_binlog``控制
- ``0``(默认): 表示每次提交事务都只write，由系统自行判断什么时候执行fsync
- ``1``: 表示每次提交事务都会执行fsync，就如同redo log 刷盘流程一样
- ``N>1``: 每次提交事务都write，但累积N个事务后才fsync。
  

**binlog与redolog对比**：
- redo log 是**物理日志**，记录内容是“在某个数据页上做了什么修改”，属于 InnoDB 存储引擎层产生的。
- 而 binlog 是**逻辑日志**，记录内容是语句的原始逻辑，类似于“给 ID=2 这一行的 c 字段加 1”，属于MySQL Server 层。
- 虽然它们都属于持久化的保证，但是则重点不同。
  - redo log让InnoDB存储引擎拥有了崩溃恢复能力。
  - binlog 保证了MySQL集群架构的数据一致性。 

**两阶段提交**：
redo log与binlog的**写入时机**不一样。redo log在事务执行过程中可以不断写入，而binlog只有在提交事务时才写入。
为了解决两份日志之间的逻辑一致问题，InnoDB存储引擎使用**两阶段提交方案**。
![](images/log_binlog_redolog.png)

<div align=center>
   <img src="images/log_binlog_redolog2.png" width = "70%" height = "70%">
</div>

<h4 align="left">中继日志(relay log)</h4>

**中继日志只在主从服务器架构的从服务器上存在**。从服务器为了与主服务器保持一致，要从主服务器读取二进制日志的内容，并且把读取到的信息写入**本地的日志文件**中，这个从服务器本地的日志文件就叫**中继日志**。然后，从服务器读取中继日志，并根据中继日志的内容对从服务器的数据进行更新，完成主从服务器的**数据同步**。

### 主从复制
在实际工作中，我们常常将Redis 作为缓存与MySQL配合来使用，当有请求的时候，首先会从缓存中进行查找，如果存在就直接取出。如果不存在再访问数据库，同时同步到Redis，这样就提升了读取的效率，也减少了对后端数据库的访问压力。
一般应用对数据库而言都是“读多写少”，也就说对数据库读取数据的压力比较大，有一个思路就是采用数据库集群的方案，做**主从架构**、进行**读写分离**。 
如果我们的目的在于提升数据库高并发访问的效率，那么首先考虑的是如何**优化SQL和索引**，这种方式简单有效；其次才是采用**缓存的策略**，比如使用 Redis将热点数据保存在内存数据库中，提升读取的效率；最后才是对数据库采用**主从架构**，进行读写分离。

**主从复制的作用：**
1. **读写分离**：我们可以通过主从复制的方式来同步数据，然后通过读写分离提高数据库并发处理能力。
![](images/master_slave.png)
2. **数据备份**:我们通过主从复制将主库上的数据复制到了从库上，相当于是一种**热备份机制**，也就是在主库正常运行的情况下进行的备份，不会影响到服务。
3. **具有高可用性**: 数据备份实际上是一种冗余的机制，通过这种冗余的方式可以换取数据库的高可用性，也就是当服务器出现故障或宕机的情况下，可以切换到从服务器上，保证服务的正常运行。

#### 主从复制的原理

Slave 会从 Master 读取 binlog 来进行数据同步。
实际上主从同步的原理就是基于 binlog 进行数据同步的。在主从复制过程中，会基于**3个线程**来操作，一个主库线程，两个从库线程。
![](images/master_slave2.png)
- **二进制日志转储线程** （Binlog dump thread）是一个主库线程。当从库线程连接的时候， 主库可以将二进制日志发送给从库，当主库读取事件（Event）的时候，会在 Binlog 上 加锁 ，读取完成之后，再将锁释放掉。
- 从库**I/O 线程**会连接到主库，向主库发送请求更新 Binlog。这时从库的 I/O 线程就可以读取到主库的二进制日志转储线程发送的 Binlog 更新部分，并且拷贝到本地的中继日志 （Relay log）。
- 从库**SQL 线程**会读取从库中的中继日志，并且执行日志中的事件，将从库中的数据与主库保持同步。
![](images/master_slave3.png)
> 先检查服务器是否已经开启了二进制日志。

**复制三步骤**(有延时):
- 步骤1： Master 将写操作记录到二进制日志（ binlog ）。这些记录叫做二进制日志事件(binary log events);
- 步骤2： Slave 将 Master 的binary log events拷贝到它的中继日志（ relay log ）；
- 步骤3： Slave 重做中继日志中的事件，将改变应用到自己的数据库中。 MySQL复制是异步的且串行化的，而且重启后从**接入点**开始复制。

**复制的基本原则**：
- 每个 Slave 只有一个 Master
- 每个 Slave 只能有一个唯一的服务器ID
- 每个 Master 可以有多个 Slave

#### 同步数据一致性问题
**主从同步的要求**：
- 读库和写库的数据一致(最终一致)；
- 写数据必须写到写库；
- 读数据必须到读库(不一定)；

**导致主从延迟的时间点**:
- 主库A执行完成一个事务，写入binlog，我们把这个时刻记为T1;
- 之后传给从库B，我们把从库B接收完这个binlog的时刻记为T2;
- 从库B执行完成这个事务，我们把这个时刻记为T3。
  - 从库的机器性能比主库要差
  - 从库的压力大
  - 大事务的执行

**减少主从延迟**：
- 降低多线程大事务并发的概率，优化业务逻辑
- 优化SQL，避免慢SQL， 减少批量操作 ，建议写脚本以update-sleep这样的形式完成。
- 提高从库机器的配置 ，减少主库写binlog和从库读binlog的效率差。
- 尽量采用 短的链路 ，也就是主库和从库服务器的距离尽量要短，提升端口带宽，减少binlog传输的网络延时。
- 实时性要求的业务读强制走主库，从库只做灾备，备份。
  
**解决一致性问题**：
1. **异步复制**：异步模式就是客户端提交COMMIT之后不需要等从库返回任何结果，而是直接将结果返回给客户端，这样做的好处是不会影响主库写的效率，但可能会存在主库宕机，而Binlog还没有同步到从库的情况，也就是此时的主库和从库数据不一致。这种复制模式下的数据一致性是最弱的。
2. **半同步复制**：在客户端提交COMMIT之后不直接将结果返回给客户端,而是等待至少有一个从库接收到了Binlog，并且写入到中继日志中，再返回给客户端。
![](images/master_slave4.png)
1. **组复制**: 组复制技术，简称 MGR（MySQL Group Replication）。首先我们将多个节点共同组成一个复制组，在**执行读写（RW）事务**的时候，必须要经过组里节点数量需要大于 （N/2+1）Node的同意，而不是原发起方一个说了算。而针对**只读（RO）事务**则不需要经过组内同意，直接 COMMIT 即可。

### 数据库的备份与恢复

- **物理备份**：备份数据文件，转储数据库物理文件到某一目录。物理备份恢复速度比较快，但占用空间比较大，MySQL中可以用``xtrabackup``工具来进行物理备份。
- **逻辑备份**：对数据库对象利用工具进行导出工作，汇总入备份文件内。逻辑备份恢复速度慢，但占用空间小，更灵活。MySQL 中常用的逻辑备份工具为``mysqldump``。逻辑备份就是**备份sql语句**，在恢复的时候执行备份的sql语句实现数据库数据的重现。

#### mysqldump实现逻辑备份

1. 备份一个数据库
    ```shell
    mysqldump -uroot -p atguigu>atguigu.sql #备份文件存储在当前目录下
    mysqldump -uroot -p atguigudb1 > /var/lib/mysql/atguigu.sql #绝对路径
    ```
2. 备份全部数据库
    ```shell
    mysqldump -uroot -pxxxxxx --all-databases > all_database.sql
    mysqldump -uroot -pxxxxxx -A > all_database.sql
    ```
3. 备份部分数据库：使用 --databases 或 -B 参数
    ```shell
    mysqldump -uroot -p --databases atguigu atguigu12 >two_database.sql
    mysqldump -uroot -p -B atguigu atguigu12 > two_database.sql
    ```
4. 备份部分表
   ``mysqldump –u user –h host –p 数据库的名称 [表名1 [表名2...]] > 备份文件名称.sql``
    ```sql
    #备份多张表
    mysqldump -uroot -p atguigu book account > 2_tables_bak.sql
    ```
5. 备份单表的部分数据
    ```shell
    mysqldump -uroot -p atguigu student --where="id < 10 " > student_part_id10_low_bak.sql
    ```
6. 排除某些表的备份
    如果我们想备份某个库，但想排除掉某些表，选项 ``--ignore-table`` 可以完成这个功能。
    ```shell
    mysqldump -uroot -p atguigu --ignore-table=atguigu.student > no_stu_bak.sql
    ```
7. 只备份结构或只备份数据
    只备份结构的话可以使用``--no-data``简写为``-d``选项；只备份数据可以使用``--no-create-info``简写为``-t``选项。
    ```shell
    #只备份结构
    mysqldump -uroot -p atguigu --no-data > atguigu_no_data_bak.sql
    #只备份数据
    mysqldump -uroot -p atguigu --no-create-info > atguigu_no_create_info_bak.sql
    ```
8. 备份中包含存储过程、函数、事件
    可以使用``--routines``或``-R``选项来备份存储过程及函数，使用``--events``或``-E``参数来备份事件。
    ```shell
    mysqldump -uroot -p -R -E --databases atguigu > fun_atguigu_bak.sql
    ```
9.  mysqldump常用选项: 运行帮助命令``mysqldump --help``查看
  
#### mysql命令逻辑恢复数据
1. 单库备份中恢复单库
    如果备份文件中包含了创建数据库的语句，则恢复的时候不需要指定数据库名称，如下所示
    ```shell
    mysql -uroot -p < atguigu.sql
    ```
    否则需要指定数据库名称，如下所示
    ```shell
    mysql -uroot -p atguigu4< atguigu.sql
    ```
2. 全量备份恢复
    如果我们现在有昨天的全量备份，现在想整个恢复
    ```shell
    mysql –u root –p < all.sql
    ```
3. 从全量备份中恢复单库
    ```shell
    sed -n '/^-- Current Database: `atguigu`/,/^-- Current Database: `/p' all_database.sql > atguigu.sql
    #分离完成后我们再导入atguigu.sql即可恢复单个库
    ```
4. 从单库备份中恢复单表
    ```shell
    #表结构
    cat atguigu.sql | sed -e '/./{H;$!d;}' -e 'x;/CREATE TABLE `class`/!d;q' >
    class_structure.sql
    #表数据
    cat atguigu.sql | grep --ignore-case 'insert into `class`' > class_data.sql
    ```

#### 物理备份和恢复
**物理备份**：直接将MySQL中的数据库文件复制出来
- 方式1：备份前，将服务器停止。
- 方式2：备份前，对相关表执行``FLUSH TABLES WITH READ LOCK``操作，备份完毕后，执行``UNLOCK TABLES`` 

**物理恢复**：步骤
1. 演示删除备份的数据库中指定表的数据
2. 将备份的数据库数据拷贝到数据目录下，并重启MySQL服务器
3. 查询相关表的数据是否恢复。需要使用下面的``chown``操作。

#### 表的导出与导入
**表的导出**
1. 使用SELECT…INTO OUTFILE导出文本文件
    mysql默认对导出的目录有权限限制
    ```sql
    mysql> SHOW GLOBAL VARIABLES LIKE '%secure%';
    +--------------------------+-----------------------+
    | Variable_name | Value |
    +--------------------------+-----------------------+
    | require_secure_transport | OFF |
    | secure_file_priv | /var/lib/mysql-files/ |
    +--------------------------+-----------------------+
    2 rows in set (0.02 sec)
    ```
    参数secure_file_priv的可选值和作用分别是:
    - 如果设置为empty，表示不限制文件生成的位置，这是不安全的设置；
    - 如果设置为一个表示路径的字符串，就要求生成的文件**只能放在这个指定的目录，或者它的子目录**(mysql默认)；
    - 如果设置为NULL，就表示禁止在这个MySQL实例上执行select \( \ldots \) into outfile 操作。
    
    例子
    ```sql
    SELECT * FROM account INTO OUTFILE "/var/lib/mysql-files/account.txt";
    ```
2. 使用mysqldump命令导出文本文件
    举例：使用mysqldump命令将将atguigu数据库中account表中的记录导出到文本文件：
    ```shell
    mysqldump -uroot -p -T "/var/lib/mysql-files/" atguigu account
    ```
3. 使用mysql命令导出文本文件
    举例：使用mysql语句导出atguigu数据中account表中的记录到文本文件：
    ```shell
    mysql -uroot -p --execute="SELECT * FROM account;" atguigu> "/var/lib/mysql-files/account.txt"
    ```

**表的导入**
1. 使用LOAD DATA INFILE方式导入文本文件
  使用SELECT...INTO OUTFILE将atguigu数据库中account表的记录导出到文本文件
    ```sql
    LOAD DATA INFILE '/var/lib/mysql-files/account_0.txt' INTO TABLE atguigu.account;
    ```
2. 使用mysqlimport方式导入文本文件
  使用mysqlimport命令将account.txt文件内容导入到数据库atguigu的account表中：
    ```shell
    mysqlimport -uroot -p atguigu '/var/lib/mysql-files/account.txt'
    ```

#### 数据库迁移
数据迁移（data migration）是指选择、准备、提取和转换数据，并**将数据从一个计算机存储系统永久地传输到另一个计算机存储系统的过程**。此外，**验证迁移数据的完整性**和**退役原来旧的数据存储**，也被认为是整个数据迁移过程的一部分。迁移包括备份与恢复。
![](images/migration.png)
包括几种迁移类型：相同版本的数据库之间迁移，不相同版本的数据库之间迁移，不同数据库之间迁移（MySQL与Oracle）

#### 安全删除
边用边总结。。。
























