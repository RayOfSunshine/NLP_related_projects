

## 神策数据分析

+ 神策入口：https://sa.boohee.cn/
+ sql教程：极客时间
+ 神策截止时间：6月19日



1. 问题：

- [ ] I/O操作是什么？

- [x] DBMS是什么？数据库管理系统

+ 需要考虑CPU计算，和内存使用情况
+ OLTP（联机事务处理过程）、OLAP（联机分析处理过程）、RDBMS（对象关系型数据库管理系统）

+ 用于 JSON 的 SQL 等
+ SQL 在不同的数据库管理系统中如何使用
+ 如何快速定位 SQL 的性能问题



2. 了解SQL

+ 数据库管理系统，最具中台能力的语言就是SQL

+ 半衰期很长的语言，通用性强，变化相对小，上手容易

+ 功能划分：

  + DDL：数据定义语言（数据库，数据表和列），可以用来创建，删除和修改数据库和表结构
  + DML：数据操作语言，用来操作和数据库相关的记录，增加，删除，修改表中的记录
  + DCL：数据控制语言，定义访问权限和安全级别
  + DQL：数据查询语言，用于查询记录

+ 声明性语言

+ ER图（实体关系图）

  + 实体：管理对象
  + 属性：实体属性
  + 关系：实体关系

+ 大小写问题：

  + <font color=red>表名，表别名，字段名，字段别名都用小写</font>

+ <font color=red>SQL 保留字、函数名、绑定变量等都大写</font>



3. 使用DDL创建数据库和数据表时应注意什么？

+ [x] DDL的基础语法：如何定义数据库和数据表？

  + DDL，定义数据库和数据表的结构
  + CREATE（增），DROP（删），改（ALTER）
  + 创建表结构：

  ```sql
  # 对数据库进行定义
  CREATE DATABASE nba; // 创建一个名为nba的数据库
  DROP DATABASE nba; // 删除一个名为nba的数据库
  
  # 创建数据表
  CREATE TABLE [table_name](字段名 数据类型，......)
  
  # 语句最后以分号作为结束符，最后一个字段的定义结束后没有逗号
  # 数据类型中 int(11) 代表整数类型，显示长度为 11 位，括号中的参数 11 代表的是最大有效显示长度
  # varchar(255)代表的是最大长度为 255 的可变字符串类型
  # NOT NULL表明整个字段不能是空值，是一种数据约束。
  # AUTO_INCREMENT代表主键自动增长。从1开始，每次增加1
  CREATE TABLE player  (
    player_id int(11) NOT NULL AUTO_INCREMENT, （player_id是主键）
    player_name varchar(255) NOT NULL
  );
  ```

  推荐使用`Navicat`：它是一个数据库管理和设计工具，跨平台，支持很多种数据库管理软件

  + 中文字符编码是 `utf8`，排序规则是`utf8_general_ci`，代表对大小写不敏感，如果设置为`utf8_bin`，代表对大小写敏感
  + 使用 `PRIMARY KEY` 规定主键
  + 在设置字段索引时，我们可以设置为 `UNIQUE INDEX`（唯一索引），也可以设置为其他索引方式，比如`NORMAL INDEX`（普通索引）
  + 在索引方式上，可以选择BTREE或者HASH
  + 存储规则采用 InnoDB，它是 MySQL5.5 版本之后默认的存储引擎，行格式为Dynamic
  + 修改表结构：

  ```sql
  # 添加字段（添加agg字段）
  ALTER TABLE player ADD (age int(11));
  
  # 修改字段名（将age字段修改为player_age）
  ALTER TABLE player RENAME COLUMN age to player_age
  
  # 修改字段的数据类型（将player_age的数据类型设置为float(3,1)）
  ALTER TABLE player MODIFY (player_age float(3,1));
  
  # 删除字段（删除刚才添加的player_age字段）
  ALTER TABLE player DROP COLUMN player_age;
  ```

+ [x] 定义数据表时，有哪些约束？

  + 字段约束（保证 RDBMS 里面数据的准确性和一致性）：
    + 主键约束（唯一标识，不能重复，可由多个字段复合而成）
    + 外健约束（确保了表与表之间引用的完整性）
    + 唯一性约束（字段在表中的数值是唯一的，创建了一个约束和普通索引，保证字段正确性）
    + `NOT NULL` 约束（字段不应为空，必须有取值）
    + `DEFAULT` 约束，如果在插入数据的时候，这个字段没有取值，就设置为默认值
    + `CHECK` 约束，检查特定字段取值范围的有效性

+ [x] 设计数据库时，有哪些重要原则

  + 数据表的个数越少越好：实体和联系设计得越简洁，既方便理解又方便操作。
  + 数据表中的字段个数越少越好：设置字段个数少的前提是各个字段相互独立
  + 数据表中联合主键的字段个数越少越好：联合主键中的字段越多，占用的索引空间越大，不仅会加大理解难度，还会增加运行时间和索引空间
  + 使用主键和外键越多越好：不仅保证了数据表之间的独立性，还能提升相互之间的关联使用率。
  + <font color=red>简单可复用</font>



4. **select的相关问题**

+ [x] select查询的基础语法
+ [x] 如何排序检索数据
+ [x] 什么情况下用select *，如何提升 select 查询效率



5. **数据过滤**

+ 指定筛选条件，进行过滤，如何使用 WHERE
+ [x] 学会使用 WHERE 子句，如何使用比较运算符对字段的数值进行比较筛选
  + 你需要查看使用的 DBMS 是否支持，不同的 DBMS 支持的运算符可能是不同的，在 MySQL 中，不支持（!>）（!<）等
  + 大于小于，范围之间，是否为空值
+ [x] 如何使用逻辑运算符，进行多条件的过滤
  + 与，或，非，在指定范围内
  + 当 WHERE 子句中同时存在 OR 和 AND，括号 的时候，一般来说 () 优先级最高，其次优先级是 AND，然后是 OR。
+ [x] 学会使用通配符对数据条件进行复杂过滤
  + 刚才讲解的条件过滤都是对已知值进行的过滤，还有一种情况是我们要检索文本中包含某个词的所有数据，这里就需要使用通配符。
  + 通配符就是我们用来匹配值的一部分的特殊字符。这里我们需要使用到 LIKE 操作符。
  + 想要匹配任意字符串出现的任意次数，需要使用（%）通配符。
  + 不同的DBMS的通配符可能不一样，大小写也可能不区分
  + 如果我们想要匹配单个字符，就需要使用下划线 (_) 通配符。
  + 不过在实际操作过程中，我还是建议你尽量少用通配符，因为它需要消耗数据库更长的时间来进行匹配
  + 实际工作中，要避免全表扫描



6. SQL函数

+ [x] 什么是 SQL 函数？
  + SQL 中的函数一般是在数据上执行的，可以很方便地转换和处理数据。当我们从数据表中检索出数据之后，就可以进一步对这些数据进行操作。
+ [x] 内置的 SQL 函数都包括哪些？
  + 算术函数：对数值类型的字段进行算术运算（绝对值，取余，指定位数的四舍五入）
  + 字符串函数：字符串拼接，大小写转化，求长度以及字符串替换和截取
  + 日期函数：对数据表中的日期进行处理
  + 转换函数：转换数据之间的类型
    + cast函数：用DECIMAL(a,b)来指定，其中 a 代表整数部分和小数部分加起来最大的位数，b 代表小数位数
+ [x] 如何使用 SQL 函数对一个数据表进行操作，比如针对一个王者荣耀的英雄数据库，我们可以使用这些函数完成哪些操作？
  + 例子很多
+ [x] 什么情况下使用 SQL 函数？为什么使用 SQL 函数有时候会带来问题？
  + DBMS的版本问题
  + 大小写的规范（不同操作系统中对大小写敏感程度不一样）
    + 关键字和函数名称全部大写；
    + 数据库名、表名、字段名称全部小写；
    + SQL 语句必须以分号结尾。



7. 聚集函数

+ 对数据进行汇总的函数，输入的是一组数据的集合，输出是单个值。
+ [x] 聚集函数都有哪些，能否在一条 SELECT 语句中使用多个聚集函数；

  + COUNT总行数, MAX最大值, MIN最小值, SUM求和, AVG平均值
  + 只要聚集函数里面不加星的，都是会自动忽略值为NULL的数据行
  + MAX 和 MIN 函数也可以用于字符串类型数据的统计，如果是英文字母，则按照 A—Z 的顺序排列，越往后，数值越大。如果是汉字则按照全拼拼音进行排列。
  + 我们也可以对数据行中不同的取值进行聚集，先用 DISTINCT 函数取不同的数据，然后再使用聚集函数。
  + 可以在一条语句中使用多个聚集函数
+ [x] 如何对数据进行分组，并进行聚集统计；

  + GROUP BY
+ [x] 如何使用 HAVING 过滤分组，HAVING 和 WHERE 的区别是什么

  + HAVING 的作用和 WHERE 一样，都是起到过滤的作用，只不过 WHERE 是用于数据行，而 HAVING 则作用于分组。



8. 子查询

+ 嵌套在查询中的查询，可以进行更复杂的查询，更容易理解查询的过程
+ 又是无法直接从数据表中得到查询结果，需要从查询结果中再次进行查询，子查询就是查询结果集
+ <font color=red> 对表的字段编写说明文档 </font>
+ [x] 什么是关联子查询，什么是非关联子查询？
  + 非关联子查询：子查询从数据表中查询了数据结果，如果这个数据结果只执行一次，然后这个数据结果作为主查询的条件进行执行
    + <font color=red>关联子查询</font>：如果子查询需要执行多次，即采用循环的方式，先从外部查询开始，每次都传入子查询进行查询，然后再将结果反馈给外部，也就是嵌套执行；每次子查询中的表都用到了外部的表，每执行一次外部查询，子查询都要重新计算一次
+ [x] 子查询中有一些关键词，可以方便我们对子查询的结果进行比较。这些关键词在子查询中的作用是什么；
  + 关联子查询通常也会和 EXISTS 一起来使用，EXISTS 子查询用来判断条件是否满足，满足的话为 True，不满足为 False。
  + 集合比较子查询：与另一个查询结果集进行比较


    ```sql
    SELECT * FROM A WHERE cc IN (SELECT cc FROM B)  # 如果表A比表B大，则使用
    SELECT * FROM A WHERE EXIST (SELECT cc FROM B WHERE B.cc=A.cc)  # 如果表B比表A大，则使用
    ```

+ [x] 子查询也可以作为主查询的列
  + 需要强调的是 ANY、ALL 关键字必须与一个比较操作符一起使用  

  

9. 常用的SQL标准


  + 连接表的操作，JOIN在SQL中的重要性

  + SQL建立在关系型数据库（数据表是典型的数据结构），因为能够在各数据表之间进行连接查询

  + 表的组成是基于关系模型的，一个表就是一个关系，关系型数据库的核心之一就是连接

  + [x] 不同SQL标准下的连接定义不同，常用的SQL标准有哪些？

    + 两个标准：SQL92，SQL99
    + + 

  + [x] 从SQL标准入门，连接表的种类有哪些？

    + SQL92 连接:
      + 笛卡尔积：两个集合 X 和 Y，笛卡尔积就是 X 和 Y 的所有可能组合
      + 等值连接：两张表的等值连接就是用两张表中都存在的列进行连接
      + 非等值连接：当我们进行多表查询的时候，如果连接多个表的条件是等号时，就是等值连接，其他的运算符连接就是非等值查询。
      + 外连接（左右连接）：查询某一方不满足条件的记录，所有表分为主表和从表

      ```sql
      (+) --SQL92
      LEFT(RIGHT) JOIN ... on ... --SQL99
      ```

      + 左外连接：左边的表是主表，需要显示左边表的全部行，右侧的表是从表
      + 右外连接：右边的表是主表，需要显示右边表的全部行，左侧的表是从表
      + 自连接：对多个表进行操作，也可以对同一个表进行操作。查询条件使用了当前表的字段

    + SQL99 连接：

      + 交叉连接：笛卡尔积（CROSS JOIN）
      + 自然连接：等值连接（NATURAL JOIN）
      + ON连接：指定想要的连接条件，使用 JOIN 和 ON
      + USING连接：USING 指定了具体的相同的字段名称，你需要在 USING 的括号 () 中填入要指定的同名字段。
      + 外连接：左外连接，右外连接，全外连接（MySQL不支持）

  + [x] SQL92 与 SQL99 的区别

    + 连接操作分为三种：
      + 内连接：将多个表之间满足连接条件的数据行查询出来。它包括了等值连接、非等值连接和自连接。
      + 外连接：会返回一个表中的所有记录，以及另一个表中匹配的行。它包括了左外连接、右外连接和全连接。
      + 交叉连接：也称为笛卡尔积，返回左表中每一行与右表中每一行的组合。
    + 区别：
      + SQL92 中进行查询时，会把所有需要连接的表都放到 FROM 之后，然后在 WHERE 中写明连接的条件。而 SQL99 在这方面更灵活，它不需要一次性把所有需要连接的表都放到 FROM 之后，而是采用 JOIN 的方式，每次连接一张表，可以多次使用 JOIN 进行连接。
      + SQL99 采用的这种嵌套结构非常清爽，即使再多的表进行连接也都清晰可见。

  + [x] 在不同的DBMS中，使用连接需要注意什么？

    + 不是所有的 DBMS 都支持全外连接
    + Oracle 没有表别名 AS
    + SQLite 的外连接只有左连接
    + 控制连接表的数量（数量越多，查询性能越低）
    + 在连接时不要忘记 WHERE 语句（可以过滤掉不必要的数据行返回）
    + 使用自连接而不是子查询



10. 视图：

+ 即虚拟表，本身不具有数据。虚拟表的创建连接了一个或多个数据表
+ 视图一方面可以帮我们使用表的一部分而不是所有的表，另一方面也可以针对不同的用户制定不同的查询视图

+ [x] 什么是视图？如何创建、更新和删除视图？

  + 作为虚拟表，帮助用户封装了底层与数据表的接口。相当于是一张表或多张表的数据结果集。
  + 能够简化SQL查询，编写视图后，可以直接复用，而不需要考虑视图中包含的基础查询的细节。
  + 也可以根据需要更改数据格式，返回与底层数据格式不同的数据
  + 在大型项目中，或数据表比较复杂的情况下，它把经常查询的结果集放到虚拟表中，提升使用效率
  + 创建视图：

  ```sql
  CREATE VIEW view_name AS
  SELECT column1, column2
  FROM table
  WHERE condition
  ```

  + 创建完毕后，可以使用`select`查看这个表

  ```sql
  SELECT * FROM view_name
  ```

  + 嵌套视图：相当于创建视图A后可以将A作为需要查询的新表添加在下一次查询条件（WHERE）中

  ```sql
  CREATE VIEW view_name AS  # view_name 是新视图
  SELECT column1, column2
  FROM table
  WHERE condition  # condition 中包含新创建的视图 existed_view_name
  ```

  + 修改视图：对原有视图进行更新

  ```sql
  ALTER VIEW view_name AS
  SELECT column1, column2
  FROM table
  WHERE condition
  ```

  + 删除视图：

  ```sql
  DROP VIEW view_name
  ```

+ [x] 如何使用视图来简化我们的 SQL 操作？

  + <font color=red>利用视图完成复杂的连接</font>：将<font color=red> 经常 </font>需要查询的数据写为一个视图，之后直接通过视图进行查询，这样就将相对复杂的连接查询转化为视图查询
  + <font color=red>利用视图对数据进行格式化</font>：需要输出某个格式的内容，首先将两个表连接，需要确定关联条件，拼接方法使用concat
  + <font color=red>使用视图与计算字段</font>：完成常见的统计需求

+ [x] 视图和临时表的区别是什么，它们各自有什么优缺点？

  + 优点：
    + 安全性：修改视图不影响底层数据；能够设置不同权限，通常不对底层数据表直接操作
    + 简单清晰：简化的是查询操作
  + 缺点：
    + 更新视图时会影响底层数据
  + 临时表：
    + 是真实存在的数据表，不过它不用于长期存放数据，只为当前连接存在，关闭连接后，临时表就会自动释放。



11. 存储过程

+ 对SQL代码进行封装，可以反复利用

+ 存储过程可以说是由<font color=red> SQL 语句和流控制语句 </font>构成的语句集合，它和我们之前学到的函数一样，可以接收输入参数，也可以返回输出参数给调用者，返回计算结果。

+ [x] 什么是存储过程，如何创建一个存储过程？

  + 就是SQL语句的封装，被创建后可以像函数一样使用
  + 定义

  ```sql
  CREATE PROCEDURE 存储过程名称([参数列表])  # 包括输入参数和输出参数
  # begin和end中间的表示需要执行的语句快
  BEGIN
      需要执行的语句
  END    
  
  # 如果需要删除，则使用
  DROP PROCEDURE
  
  # 如果需要修改，则使用
  ALTER PROCEDURE
  ```

  + 在MySQL中，需要加入DELIMITER来定义新的结束符，新的结束符为//；首先用（//）作为结束符，又在整个存储过程结束后采用了（//）作为结束符号，告诉 SQL 可以执行了，然后再将结束符还原成默认的分号。
  + 3种参数类型：IN（传入参数不返回），OUT（不传入参数，返回），INOUT（既传参数又返回）

+ [x] 流控制语句都有哪些，如何使用它们？

  + BEGIN…END：中间包含多个语句
  + DECLARE：用来声明变量
  + SET：赋值语句
  + SELECT…INTO：把从数据表中查询的结果存放到变量中，也就是为变量赋值
  + IF…THEN…ENDIF：条件判断语句，我们还可以在 IF…THEN…ENDIF 中使用 ELSE 和 ELSEIF 来进行条件判断。
  + CASE：CASE 语句用于多条件的分支判断
  + LOOP、LEAVE 和 ITERATE：LOOP 是循环语句，使用 LEAVE 可以跳出循环，使用 ITERATE 则可以进入下一次循环
  + REPEAT…UNTIL…END REPEAT：这是一个循环语句，首先会执行一次循环，然后在 UNTIL 中进行表达式的判断，如果满足条件就退出，即 END REPEAT；如果条件不满足，则会就继续执行循环，直到满足退出条件为止
  + WHILE…DO…END WHILE：这也是循环语句，和 REPEAT 循环不同的是，这个语句需要先进行条件判断，如果满足条件就进行循环，如果不满足条件就退出循环。

+ [x] 各大公司是如何看待存储过程的？在实际工作中，我们该如何使用存储过程？

  + 优点：
    + 存储过程可以一次编译多次使用
    + 减少开发工作量，将代码封装成模块，保证代码结构清晰
    + 存储过程的安全性强，可以设置用户的使用权限
    + 减少网络传输量
  + 缺点：
    + 可移植性差，存储过程不能跨数据库移植
    + 调试困难
    + 版本管理困难
    + 不适合高并发的场景



12. 事务处理（transaction）

+ 要么完全执行，要么都不执行

+ 保证了一次处理的完整性，也保证了数据库中的数据一致性

+ 一种较高级的处理方式，如果在增加、删除、修改的时候某一个环节出了错，它允许我们回滚还原。

+ [x] 事务的特性是什么？如何理解它们？(ACID)

  + <font color=red> 基础 </font>原子性（Atomicity）。基本单位，不可分割
  + <font color=red> 手段 </font>一致性（Consistency）。一致性指的就是数据库在进行事务操作后，会由原来的一致状态，变成另一种一致的状态。任何写入数据库中的数据都需要满足我们事先定义的约束规则。
  + <font color=red> 约束条件 </font>隔离性（Isolation）。每个事务都是彼此独立的，不会受到其他事务的执行影响。
  + <font color=red> 目的 </font>持久性（Durability）。事务提交之后对数据的修改是持久性的。当事务完成，数据库的日志就会被更新，这时可以通过日志，让系统恢复到最后一次成功的更新状态。

+ [x] 如何对事务进行控制？控制的命令都有哪些？

  ```sql
  # 使用如下语句来查看当前支持的存储引擎，以及是否支持事务
  SHOW ENGINES
  ```

  + 控制语句：
    + START TRANSACTION 或者 BEGIN，作用是显式开启一个事务。
    + COMMIT：提交事务。当提交事务后，对数据库的修改是永久性的。
    + ROLLBACK 或者 ROLLBACK TO [SAVEPOINT]，意为回滚事务。
    + SAVEPOINT：在事务中创建保存点，方便后续针对保存点进行回滚。一个事务中可以存在多个保存点。
    + RELEASE SAVEPOINT：删除某个保存点。
    + SET TRANSACTION，设置事务的隔离级别。

  ```sql
  # 隐式事务，就是自动提交，MySQL默认自动提交，可以配置MySQL的参数
  set autocommit = 0;  
  //关闭自动提交，不论是否采用 START TRANSACTION 或者 BEGIN 的方式来开启事务，都需要用 COMMIT 进行提交，让事务生效，使用 ROLLBACK 对事务进行回滚。
  set autocommit = 1;  
  //开启自动提交，每条 SQL 语句都会自动进行提交。如果采用 START TRANSACTION 或者 BEGIN 的方式来显式地开启事务，那么这个事务只有在 COMMIT 时才会生效，在 ROLLBACK 时才会回滚。
  ```

+ [x] 为什么我们执行 COMMIT、ROLLBACK 这些命令的时候，有时会成功，有时会失败？
  + 如果显式，分开写明了若干次事务，那么rollback时，保留的是除当前报错所在的事务外的最新事务
  + 如果隐式，比如多次插入操作存在于一个事务中，由于已经设定为自动提交，所以报错回滚后，保留的是除报错信息外的结果

  ```sql
  SET @@completion_type = 1;  // 使用后结果等于显式事务
  ```

  + completion_type 参数
    + completion_type = 0，这是默认情况。也就是说当我们执行 COMMIT 的时候会提交事务，在执行下一个事务时，还需要我们使用 START TRANSACTION 或者 BEGIN 来开启。
    + completion_type = 1，这种情况下，当我们提交事务后，相当于执行了 COMMIT AND CHAIN，也就是开启一个链式事务，即当我们提交事务之后会开启一个相同隔离级别的事务，相当于在下一行写了一个 START TRANSACTION 或 BEGIN。<font color=red>后续如果不再使用提交（COMMIT），则认为后续SQL语句都属于一个事务。</font>
    + completion_type = 2，这种情况下 COMMIT=COMMIT AND RELEASE，也就是当我们提交后，会自动与服务器断开连接。



13. 事务隔离

+ 考虑到随着用户量的增多，会存在大规模并发访问的情况，这就要求数据库有更高的吞吐能力，串行化行无法满足高并发的需求，需要降低数据库的隔离标准，来换取事务之间的并发能力。

+ 有时候我们需要牺牲一定的正确性来换取效率的提升，也就是说，我们需要通过设置不同的隔离等级，以便在正确性和效率之间进行平衡。

+ [x] 事务并发处理可能存在的三种异常有哪些？什么是脏读、不可重复读和幻读？

  + 脏读（Dirty Read）：一个事务读取了另一个事务改写但还未提交的数据。
  + 不可重复读（Nonrepeatable Read）：在同一个事务中，多次读取同一数据返回的结果有所不同。
  + 幻读（Phantom Read）：一个事务读取了另一个事务插入的新纪录。

+ [x] 针对可能存在的异常情况，四种事务隔离的级别分别是什么？

  + 按照【异常数量】从少到多的顺序（脏读--不可重复读--幻读）

  + 读未提交（READ UNCOMMITTED ）
  + 读已提交（READ COMMITTED）
  + 可重复读（REPEATABLE READ）
  + 可串行化（SERIALIZABLE），也就是在一个队列中按照顺序执行，可串行化是最高级别的隔离等级

+ [ ] 如何使用 MySQL 客户端来模拟脏读、不可重复读和幻读？

  + 隔离级别越低，意味着系统吞吐量（并发程度）越大，但同时也意味着出现异常问题的可能性会更大。



14. 游标

+ SQL本身是以关系模型和集合论为基础的，所以它具有面向集合的思考过程，更关注“获取什么”，而不是“如何获取”

+ 但是有时需要处理一行或者一部分行，这是就需要面向过程的编程方法了

+ [x] 什么是游标？我们为什么要使用游标？

  + 灵活的从数据结果集中每次提取一条数据记录进行操作，是面向过程的编程方式
  + 游标是一种临时的数据库对象，可以指向存储在数据库表中的数据行指针。

+ [x] 如何使用游标？使用游标的常用步骤都包括哪些？

  + 使用游标的五个步骤：

  ```sql
  // 1. 定义游标
  DECLARE cursor_name CURSOR FOR select_statement // select_statement表示select语句
  
  // 2. 打开游标，打开游标的时候 SELECT 语句的查询结果集就会送到游标工作区。
  OPEN cursor_name
  
  // 3. 从游标中取得数据，这句的作用是使用 cursor_name 这个游标来读取当前行，并且将数据保存到 
  // var_name 这个变量中，游标指针指到下一行。如果游标读取的数据行有多个列名，则在 INTO 关键
  // 字后面赋值给多个变量名即可。
  FETCH cursor_name INTO var_name ...
  
  // 4. 关闭游标
  CLOSE cursor_name
  
  // 5. 释放游标
  DEALLOCATE cursor_namec
  ```

  + 简单例子的说明：

  ```sql
  DELIMITER //
  CREATE PROCEDURE `calc_hp_max`()
  BEGIN 
  		-- 创建接收游标的变量 
  		DECLARE hp INT; 
  		-- 创建总数变量 
  		DECLARE hp_sum INT DEFAULT 0; 
  		-- 创建结束标志变量 
  		DECLARE done INT DEFAULT false; 
  		-- 定义游标 
  		DECLARE cur_hero CURSOR FOR SELECT hp_max FROM heros; 
  		-- 指定游标循环结束时的返回值 
  		DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = true;
  		OPEN cur_hero; 
  		-- 创建循环
  		read_loop: LOOP 
  		-- 获取每一行的具体数值，并负值给变量hp
  		FETCH cur_hero INTO hp;
          -- 判断游标的循环是否结束 
          IF done THEN LEAVE read_loop; END IF;
  		-- 利用 hp_sum 做累加求和
  		SET hp_sum = hp_sum + hp;
          -- 结束循环
  		END LOOP; 
  		CLOSE cur_hero; 
  		SELECT hp_sum;
  END//
  ```

  + 运行时，可以直接使用 `CALL calc_hp_max();`进行查看
  + 除了使用 LOOP 循环以外，你还可以使用 REPEAT… UNTIL…以及 WHILE 循环。它们同样需要设置 CONTINUE 事件来处理游标溢出的情况。

+ [x] 如何使用游标来解决一些常见的问题？

  + 有的时候，我们需要找特定数据，用 SQL 查询写起来会比较困难，比如两表或多表之间的嵌套循环查找，如果用 JOIN 会非常消耗资源，效率也可能不高，而用游标则会比较高效。
  + 还有就是不同类型，数值的数据处理结果不相同，可以使用`游标+存储过程`的方式完成处理
  + 在对数据表进行更新前，需要备份之前的表。
  + 虽然在处理某些复杂的数据情况下，使用游标可以更灵活，但同时也会带来一些性能问题，比如在使用游标的过程中，会对数据行进行加锁，这样在业务并发量大的时候，不仅会影响业务之间的效率，还会消耗系统资源，造成内存不足，这是因为游标是在内存中进行的处理。如果有游标的替代方案，我们可以采用替代方案。



15. 使用python操作MySQL

+ [x] Python 的 DB API 规范是什么，遵守这个规范有什么用？
  + 数据库连接对象，数据库交互对象，数据库异常类
  + 操作DBMS时需要急性以下几个步骤：引入API模块，与数据库建立连接，执行SQL语句，关闭数据库连接
  
+ [x] 基于 DB API，MySQL 官方提供了驱动器 mysql-connector，如何使用它来完成对数据库管理系统的操作？

  ```python
  # -*- coding: UTF-8 -*-
  import mysql.connector
  # 打开数据库连接
  
  db = mysql.connector.connect(
         host="localhost",
         user="root",
         passwd="Sxty6369",    # 数据库密码
         database='heros',     # 选择一个database
         auth_plugin='mysql_native_password')
  
  # 创建游标以操作数据，之后可以通过面向过程的编程方式对数据库数据进行操作
  cursor = db.cursor()
  
  # 执行SQL语句
  cursor.execute("SELECT VERSION()")
  
  # 获取一条数据
  data = cursor.fetchone()
  
  print("MySQL版本: %s " % data)
  
  # 关闭游标&数据库连接
  cursor.close()
  db.close()
  ```

  + `connection`是对数据库的当前连接进行管理
    + 使用 `db.begin()` 开启事务；
    + 使用 `db.commit()` 和 `db.rollback()`，对事务进行提交以及回滚。
  +  `cursor` Cursor 是对数据库的游标进行管理
    + 使用 `cursor.fetchall()`，取出数据集中的所有行，返回一个元组 `tuples` 类型；
    + 使用 `cursor.fetchmany(n)`，取出数据集中的多条数据，同样返回一个元组 `tuples`；
    + 使用 `cursor.rowcount`，返回查询结果集中的行数。如果没有查询到数据或者还没有查询，则结果为 -1，否则会返回查询得到的数据行数；

+ [x] 如何完成对数据表的 CRUD 操作（增加，读取，修改和删除）？

  + 增加数据

  ```python
  # 插入新球员，无论传入的数值为整数类型还是浮点类型，都统一使用%s进行占位
  sql = "INSERT INTO player (team_id, player_name, height) VALUES (%s, %s, %s)"
  val = (1003, "约翰-科林斯", 2.08)
  cursor.execute(sql, val)  # 执行SQL语句，val为传递的参数
  db.commit()  # 进行提交
  print(cursor.rowcount, "记录插入成功。")
  ```

  + 读取数据

  ```python
  # 查询身高大于等于2.08的球员
  sql = 'SELECT player_id, player_name, height FROM player WHERE height>=2.08'
  cursor.execute(sql)
  data = cursor.fetchall()
  for each_player in data: 
      print(each_player)
  ```

  + 修改数据

  ```python
  # 修改球员约翰-科林斯
  sql = 'UPDATE player SET height = %s WHERE player_name = %s'
  val = (2.09, "约翰-科林斯")
  cursor.execute(sql, val)
  db.commit()
  print(cursor.rowcount, "记录被修改。")
  ```

  + 删除数据

  ```python
  sql = 'DELETE FROM player WHERE player_name = %s'
  val = ("约翰-科林斯",)
  cursor.execute(sql, val)
  db.commit()
  print(cursor.rowcount, "记录删除成功。")
  ```

  + 需要注意的几点：

    + 打开数据库连接以后，如果不再使用，则需要关闭数据库连接，以免造成资源浪费。
    + 在对数据进行增加、删除和修改的时候，可能会出现异常，这时就需要用try...except捕获异常信息。如：

    ```python
    import tracebacktry: 
        sql = "INSERT INTO player (team_id, player_name, height) VALUES (%s, %s, %s)" 
        val = (1003, "约翰-科林斯", 2.08) 
        cursor.execute(sql, val) 
        db.commit() 
        print(cursor.rowcount, "记录插入成功。")
    except Exception as e: 
        # 打印异常信息 
        traceback.print_exc() 
        # 回滚 
        db.rollback()
    finally: 
        # 关闭数据库连接 
        db.close()
    ```

    + 如果你在使用 mysql-connector 连接的时候，系统报的错误为authentication plugin caching_sha2，这时你需要下载最新的版本更新来解决
  
  

16. SQLAlchemy，使用python ORM框架来操作MySQL

+ 随着项目规模的增加，代码会越来越复杂，维护的成本也越来越高，这时 mysql-connector 就不够用了，我们需要更好的设计模式。

+ [x] 什么是 ORM 框架，以及为什么要使用 ORM 框架？
  + 持久化，在业务逻辑层和数据库层起到了衔接的作用，能够将内存中的数据模型与存储模型互相转化
    + 持久性就是将对象数据永久存储在数据库中
    + <font color=red>通常我们将数据库的作用理解为永久存储，将内存理解为暂时存储。</font>
    + 我们在程序的层面操作数据，其实都是把数据放到内存中进行处理，如果需要数据就会通过持久化层，从数据库中取数据；如果需要保存数据，就是将对象数据通过持久化层存储到数据库中。
  + ORM (object relation mapping)，对象关系映射：
    + 提供了一种持久化模式，可以把底层的 RDBMS 封装成业务实体对象，提供给业务逻辑层使用。
    + 采用 ORM，就可以从数据库的设计层面转化成面向对象的思维。
    + 采用基于 ORM 的方式来操作数据库，好处就是一旦定义好了对象模型，就可以让它们简单可复用，从而不必关注底层的数据库访问细节，只要将注意力集中到业务逻辑层面。
    + 即便数据库本身进行了更换，在业务逻辑代码上也不会有大的调整。这是因为 ORM 抽象了数据的存取，同时也兼容多种 DBMS
    + 面对一些复杂的数据查询，ORM 会显得力不从心。虽然可以实现功能，但相比于直接编写 SQL 查询语句来说，ORM 需要编写的代码量和花费的时间会比较多
+ [x] Python 中的 ORM 框架都有哪些？
  + Django，包括了 Model（模型），View（视图）和 Template（模版）。Model 模型只是 Django 的一部分功能，我们可以通过它来实现数据库的增删改查操作。
  + SQLALchemy，如果你想用支持 ORM 和支持原生 SQL 两种方式的工具，那么 SQLALchemy 是很好的选择。
  + peewee，这是一个轻量级的 ORM 框架，简单易用。peewee 采用了 Model 类、Field 实例和 Model 实例来与数据库建立映射关系，从而完成面向对象的管理方式。
+ [x] 如何使用 SQLAlchemy 来完成与 MySQL 的交互？
  + create_engine
    + 需要提供数据库 + 数据库连接框架，即对应的是mysql+mysqlconnector
  + 创建相应的模型
    + `__tablename__` 指明了模型对应的数据表名称
    + 在 Player 模型中对采用的变量名进行定义，变量名需要和数据表中的字段名称保持一致，否则会找不到数据表中的字段。采用 Column 对字段进行定义。
  + 数据表进行增删改查：
    + 增
      + 首先需要初始化 DBSession，相当于创建一个数据库的会话实例 session，通过上一步创建的模型完成
    + 查
      + 对整个数据行进行查询，采用的是session.query(Player)，相当于使用的是 SELECT *
      + 这时如果我们想要在 Python 中对 query 结果进行打印，可以对 Base 类增加to_dict()方法，相当于将对象转化成了 Python 的字典类型。
      + 在进行查询的时候，我们使用的是 filter 方法，对应的是 SQL 中的 WHERE 条件查询。除此之外，filter 也支持多条件查询。
      + 注意`AND` 和`OR`的写法
      + 分组操作，sqlalchemy 的 func 类，它提供了各种聚集函数，比如 func.count 函数
      + 在 query() 后面使用了 group_by() 进行分组，再使用 having 对分组条件进行筛选
      + 使用 order_by 进行排序
    + 删
      + 需要先进行查询，然后再从 session 中把这些数据删除掉
      + 判断球员姓名是否为约翰·科林斯，这里需要使用（==）
    + 改
      + 如果我们想要修改某条数据，也需要进行查询，然后再进行修改。



17. 答疑：

+ 降低系统 I/O

  + 列式存储是把一列的数据都串起来进行存储，然后再存储下一列。这样做的话，相邻数据的数据类型都是一样的，更容易压缩，压缩之后就自然降低了 I/O。
  + OLTP：
    + 一般用于处理客户的事务和进行查询，需要随时对数据表中的记录进行增删改查，对实时性要求高采用行式存储
  + OLAP：
    + 一般用于市场的数据分析，通常数据量大，需要进行复杂的分析操作，可以对大量历史数据进行汇总和分析，对实时性要求不高。适合采用列式存储，汇总数据非常快，但是对于插入（INSERT）和更新（UPDATE）性能会差不少。

+ COUNT(*)， COUNT(1)，COUNT(具体字段) 的查询效率？

  + 在 MySQL InnoDB 存储引擎中，COUNT(*)和COUNT(1)都是对所有结果进行COUNT。本质上并没有区别，执行的复杂度都是O(N)。
  + 如果采用COUNT(*)和COUNT(1)来统计数据行数，要尽量采用二级索引
  + 如果想要查找具体的行，那么采用主键索引的效率更高
  + COUNT(*)= COUNT(1)> COUNT(字段)，尽量使用 COUNT(\*)

+ 已经通过 WHERE 条件过滤得到了数据，为什么在 ORDER BY 字段上还要加索引呢？

  + 在 Index 排序中，索引可以保证数据的有序性，不需要再进行排序，效率更高。
  + FileSort 排序则一般在内存中进行排序，占用 CPU 较多，效率较低。

  + 优化建议：
    + 可以在 WHERE 子句和 ORDER BY 子句中使用索引，目的是在 WHERE 子句中避免全表扫描，在 ORDER BY 子句避免使用 FileSort 排序。
    + 尽量使用 Index 完成 ORDER BY 排序。如果 WHERE 和 ORDER BY 后面是相同的列就使用单索引列；如果不同就使用联合索引。

+ ORDER BY 就是对记录进行排序。

+ SELECT 语句内部的执行步骤

  ```sql
  FROM 子句组装数据（包括通过 ON 进行连接）
  WHERE 子句进行条件筛选；
  GROUP BY 分组 ；
  使用聚集函数进行计算；
  HAVING 筛选分组；
  计算所有的表达式；
  SELECT 的字段；
  ORDER BY 排序；
  LIMIT 筛选。
  ```

+ 解哪种情况下应该使用 EXISTS，哪种情况应该用 IN:
  + 选择的标准理解为小表驱动大表

+ 使用存储过程声明变量时，支持的数据类型？
  + 常见的数据类型可以分成三类，分别是数值类型、字符串类型和日期／时间类型。
+ 存储过程定义中是否一定要包含 IN 参数？
  + 如果存储过程定义了 IN 参数，就需要在调用的时候传入。当然在定义存储过程的时候，如果不指定参数类型，就默认是 IN 类型的参数。在定义中可以省略不写。
  + 在存储过程中的语句里，不一定要用到 IN 参数，只是在调用的时候需要传入这个。
+ 事务处理相关：
  + 在 MySQL 中 BEGIN 用于开启事务，如果是连续 BEGIN，当开启了第一个事务，还没有进行 COMMIT 提交时，会直接进行第二个事务的 BEGIN，这时数据库会隐式地 COMMIT 第一个事务，然后再进入到第二个事务。
  + ROLLBACK 是针对当前事务的
  + 开发者可以决定，如果遇到了小错误是直接忽略，提交事务，还是遇到任何错误都进行回滚。如果我们强行进行 COMMIT，数据库会将这个事务中成功的操作进行提交，它会认为你觉得已经是 ACID 了