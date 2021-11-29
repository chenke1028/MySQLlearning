### MySQL必知必会

#### 第一章 了解SQL

##### 1.1 数据库基础

* DBMS 数据库管理系统：即人们常用的数据库管理软件

* database：数据库，保存有组织的数据的容器

* 表：存放某种特定类型的数据的文件（结构化清单）

* 列：即字段，保存一类特定的信息，具有相应的数据类型

* 行：有时候被称为一条记录

* 主键：一列或一组列，值能唯一区分表中的每个行

  主键要满足的条件：

  1. 任意两行都不具有相同的主键值
  2. 每个行都必须有主键值（非NULL)

  建立主键应该有的几个好习惯：不更新主键列中的值，不重用主键列的值，不使用可能会更改的值

##### 1.2 什么是SQL

Structed Query Language 结构化查询语言，是一种专门用来与数据库通信的语言

每个DBMS都有其特定的SQL语句，但是总体上是类似的

#### 第二章 MySQL简介

##### 2.1 什么是MySQL

MySQL是一种DBMS，具有开源的特性，并且不断地在进行更新，增加新特性

###### 2.1.1 客户机-服务器软件

MySQL是基于客户机-服务器的数据库软件，服务器是负责数据处理与访问的软件，这个软件运行在称为数据库服务器的计算机上

只有服务器软件与数据文件打交道，关于数据的所有请求都由服务器软件完成，这些请求来自于运行客户机软件的计算机。

实际上，客户机与服务器软件可能安装在两台计算机或一台计算机上，不管有几台，为了进行数据库交互，客户及软件都要与服务器软件进行通信

###### 2.1.2 MySQL版本

目前MySQL更新到5版本，本机使用的是5.7版

##### 2.2 MySQL命令行

* 命令用";"结尾，所以单独的Enter并不运行命令
* 输入help可以获得帮助，输入`help select`这样的语句可以得到具体关于SELECT语句的帮助

#### 第三章 使用MySQL

##### 3.1 连接

需要登陆

##### 3.2 选择数据库

使用 `USE` 关键字

如要访问crashcourse这个数据库，可以使用下列语句

```sql
USE crashcourse
```

##### 3.3 了解数据库和表

如果想显示可用的数据库列表或者数据库的表列表以及表的行与列，可以使用 `SHOW `关键字

```sql
SHOW　DATABASES;
SHOW　TABLES;
SHOW COLUMNS FROM customers;
```

`DESCRIBE` 语句，可以作为一种快捷语句

如：

```sql
DESCRIBE customers;
```

这一句等同于：

```sql
SHOW COLUMNS FROM customers;
```

#### 第四章 检索数据

##### 4.1 SELECT 语句

可以从一个或者多个表中检索数据的语句，必须至少包含两条信息：想选择什么，从什么地方选择

##### 4.2 检索单个列

```sql
SELECT prod_name
FROM products;
```

从products表中检索了prod_name整个列

> 这些数据是未经排序的，可能是数据添加到表中的数据，也可能不是，默认这些数据没特殊意义即可

> sql语句本身是没有大小写区分的，但是为了阅读与调试的方便，开发人员通常喜欢大写关键字，

##### 4.3 检索多个列

选择多个列时，一定要在列名的后面加上','来进行分隔

如：

```sql
SELECT prod_id,prod_name,prod_price
FROM products;
```

##### 4.4 检索所有列

使用通配符（*)来达到效果，如：

```sql
SELECT * FROM products
```

##### 4.5 检索不同的行

有时候，我们会想得到整个表中不同的数据，如在product表中检索出不同的供应商id，此时我们要用`DISTINCT`关键字

如：

```sql
SELECT DISTINCT vend_id
FROM products;
```

> 不能部分使用DISTINCT 如果使用了DISTINCT关键字，那么它将应用于所有的列而不是前置它的列，如果给出SELECT DISTINCT vend_id,prod_price,除非指定的两个列都不同，否则将检索出所有的行

##### 4.6 限制结果

 `SELECT`语句可以使用`LIMIT`子句来返回所检索记录的第一行或前几行

如：

```sql
SELECT prod_nameFROM productsLIMIT 5;
```

这句sql语句表示返回的记录不超过5条

```sql
SELECT prod_nameFROM productsLIMIT 5,5;
```

这句sql表示从行5开始的5行

> 行0：检索出来的第一行为行0，所以行5表示的是第6行而不是第5行

> 在行数不够时，如 LIMIT 10，5 那么mysql将只返回它所能返回的那么多行

> MySQL 5的替代语法 `LIMIT 3,4`==`LIMIT 4 OFFSET 3`

##### 4.7 使用完全限定的表名

可以同时使用完全限定的名字来限定列（同时包含表名与列名）

如：

```sql
SELECT products.prod_nameFROM products;
```

同样的，表名也可以完全限定，

如：

```sql
SELECT products.prod_nameFROM crashcourse.products;
```

#### 第五章 排序检索数据

##### 5.1 排序数据

在前文的检索中，所检索的数据都是未经排序的

我们可以使用`ORDER BY`子句来对输出进行排序

如：

```sql
SELECT prod_nameFROM productsORDER BY prod_name;
```

 

> 事实上，我们也可以使用未被选择的列进行排序，这是合法的

##### 5.2 按多个列排序

如果希望按照多个列进行排序，只要在列名之间用逗号分隔即可

如：

```sql
SELECT prod_id,prod_price,prod_nameFROM productsORDER BY prod_price,prod_name;
```

##### 5.3 指定排序方向

在默认的情况下，mysql进行的排序是升序排序，但是我们也可以通过 `DESC`关键字来改变排序顺序，

如下列代码，以价格降序排列商品

```sql
SELECT prod_id,prod_price,prod_nameFROM productsORDER BY prod_price DESC;
```

如果想对多个列排序，那么代码如下：

```sql
SELECT prod_id,prod_price,prod_nameFROM productsORDER BY prod_price DESC,prod_name;
```

> 在上述例子中，只有prod_price是进行的降序排序，prodname仍然是升序，如果想对多个列进行降序排序，必须在每个列名后加上DESC关键字

> 字母大小写的排序问题，见书p42，通过sql语句做不到，要依靠管理员

组合子句：

我们可以进行`LIMIT`与`ORDER BY`的组合，从而找出最贵物品的值，

如：

```sql
SELECT prod_priceFROM productsORDER BY prod_price DESCLIMIT 1;
```

> ORDER BY子句必须保证在FROM之后，LIMIT之前

#### 第六章 过滤数据

##### 6.1 使用WHERE子句

我们可以通过`WHERE`子句来对数据进行过滤（限定条件）

WHERE子句在FROM语句之后

如：

```sql
SELECT prod_name,prod_priceFROM productsWHERE prod_price=2.50;
```

> ORDER BY 子句应该在WHERE子句之后

##### 6.2 条件操作符

等于：‘=’ ；不等于：‘！=’，‘<>'; 小于：'<'; 小于等于：’<=‘; 大于：’>'; 大于等于：’>='; BETWEEN:在两个值之间；

对于WHERE子句 书上有检查单个值，不匹配检查，范围值检查，空值检查多个例子，

见p38即可，值得注意的是空值检查，此时需要的是 `IS NULL`子句

#### 第七章 数据过滤

##### 7.1 组合WHERE子句

我们可以使用`AND`与`OR`操作符来达成更复杂的操作检索，如：

```sql
SELECT prod_id,prod_price,prod_nameFROM productsWHERE vend_id=1003 AND prod_price <=10;
```

```sql
SELECT prod_name,prod_priceFROM productsWHERE vend_id=1002 OR vend_id=1003;
```

对于同时使用AND与OR关键字时，书上在p42给出了对于计算次序的分析，通常情况下，最好使用圆括号明确的分组相应的操作符，如：

```sql
SELECT prod_name,prod_priceFROM productsWHERE(vend_id=1002 OR vend_id=1003) AND prod_price >=10;
```

##### 7.2 IN操作符

IN操作符可以指定一个范围，作为检索匹配的指定清单，其中值用逗号分隔符进行分隔

如：

```sql
SELECT prod_name,prod_priceFROM productsWHERE vend_id IN (1002,1003)ORDER BY prod_name;
```

`IN`子句可以包含其它`SELECT`语句，可以更动态的建立WHERE子句，第14章将进行更多的介绍

##### 7.3 NOT操作符

我们可以使用`not`操作符来对IN，BETWEEN，EXISTS子句进行取反，如

```sql
SELECT prod_name,prod_priceFROM productsWHERE vend_id NOT IN(1002,1003)ORDER BY prod_name;
```

> 在通常情况下，NOT子句确实没有什么优势，但是在复杂的情况下，我们可以对IN子句进行取反，从而快速排除与条件不匹配的行

#### 第八章 用通配符进行过滤

##### 8.1 LIKE操作符

###### 8.1.1 百分号%通配符

百分号%通配符表示任意字符出现任意次数

如：

```sql
SELECT prod_id,prod_nameFROM productsWHERE prod_name LIKE 'jet%'
```

可以搜索所有产品名为jet开头的产品（大小写不敏感）

而：

```sql
SELECT prod_id,prod_nameFROM productsWHERE prod_name LIKE '%anvil%'
```

可以表示搜索到任意产品名包含"anvil"的产品

通配符还可以出现在搜索模式的中间位置，如：

```sql
SELECT prod_id,prod_nameFROM productsWHERE prod_name LIKE 's%e'
```

表示找到以's'开头'e'结尾作为产品名的产品

> 值得注意的是，%可以匹配0个字符

###### 8.1.2 下划线(_)通配符

(_)下划线通配符也表示任意字符，但是仅表示一个任意字符的意思

如：

```sql
SELECT prod_id,prod_nameFROM productsWHERE prod_name LIKE '_ ton anvil';
```

会返回：

```sql
+---------+-------------+| prod_id | prod_name   |+---------+-------------+| ANV02   | 1 ton anvil || ANV03   | 2 ton anvil |+---------+-------------+
```

而

```sql
SELECT prod_id,prod_nameFROM productsWHERE prod_name LIKE '% ton anvil';
```

会返回

```sql
+---------+--------------+| prod_id | prod_name    |+---------+--------------+| ANV01   | .5 ton anvil || ANV02   | 1 ton anvil  || ANV03   | 2 ton anvil  |+---------+--------------+
```

我们可以通过这个例子来理解两个通配符的区别



#### 第九章 正则表达式

##### 9.1 正则表达式介绍

正则表达式是用来匹配文本的特殊的串，正则表达式使用正则表达式语言，得学习特殊的语法与指令。

##### 9.2 MySQL的正则表达式

> MySQL仅支持部分的正则表达式

###### 9.2.1 基本字符匹配

下例将展示基本的正则表达式的用法

```sql
SELECT prod_nameFROM productsWHERE prod_name REGEXP '1000'ORDER BY prod_name;
```



表示的是检索prod_name内包含1000这个字符串的产品名

在下列情况下，正则表达式的优点展现的更多

```sql
SELECT prod_nameFROM productsWHERE prod_name REGEXP '.000'ORDER BY prod_name;
```

`.`字符是正则表达式中的一个特殊字符，表示匹配任意一个字符

> 使用正则表达式时，若想区分大小写，可以使用`BINARY`关键字

###### 9.2.2 进行OR匹配

搜索两个串之一，可以使用`|`运算符，表示或的概念

如：

```sql
SELECT prod_nameFROM productsWHERE prod_name REGEXP '1000|2000'ORDER BY prod_name;
```

这个语句表示1000和2000都返回

> 可以使用两个以上的OR条件，如'1000|2000|3000'

###### 9.2.3 匹配几个字符之一

如果想匹配几个字符之一，可以使用`[]`运算符，如：

```sql
SELECT prod_nameFROM productsWHERE prod_name REGEXP '[123] Ton'ORDER BY prod_name;
```

> 此处容易有对于集合的误区，事实上，[]运算符作为集合的标志，在大多数情况下是必要的

> 集合是可以被否定的，我们可以使用`^`运算符如[^123]
>
> 表示除123之外的任何东西

###### 9.2.4 匹配范围

为了简化范围如`[0123456789]`这个表达可以被`[0-9]`代替，同样的，也可以用`[a-z]`来表示一个字母的范围

###### 9.2.5 匹配特殊字符

对于`.`这个特殊字符，如果用`REGEXP '.'`作为搜索条件的话，会检索出所有的行数，因为正则表达式中，`.`表示任一字符

为了匹配特殊字符，我们必须使用`\\`作为前缀

这被成为转义处理

###### 9.2.6 匹配字符类

在书p58页上，列出了一个匹配字符类的表

###### 9.2.7 匹配多个实例

同样见书

###### 9.2.8 定位符

见书

#### 第十章 创建计算字段

##### 10.1 计算字段

在检索过程中，我们常常会遇见需要的格式与数据库存储形式不符合的情况，（见p62）

> 计算字段并不存在于数据库表中，而是在运行时在SELECT语句内生成的

##### 10.2 拼接字段

在SELECT语句中，我们可以使用`Concat()`函数来拼接两个列

如：

```sql
SELECT Concat(vend_name,'(',vend_country,')')FROM vendorsORDER BY vend_name;
```

此时SELECT语句返回的就是计算字段

我们曾提到过用函数来删除空格，由下例子来阐述：

```sql
SELECT Concat(RTrim(vend_name),'(',RTrim(vend_country),')')FROM vendorsORDER BY vend_name;
```

> RTrim表示去掉右侧的空格，而LTrim表示去除左侧的空格，Trim则是两端的空格

为了方便客户机的使用，我们可以使用`AS`关键字来创建计算字段为一个别名

如：

```sql
SELECT Concat(vend_name,'(',vend_country,')') AS vend_titleFROM vendorsORDER BY vend_name;
```

##### 10.3 执行算术运算

我们在SELECT语句中，还可以使用算术运算符构建计算字段

如：

```sql
SELECT prod_id,	   quantity,	   item_price,	   quantity*item_price AS expanded_priceFROM orderitemsWHERE order_num=20005;
```



#### 第十一章 使用数据处理函数

##### 11.1 函数

SQL支持函数来处理数据，上一章中的Trim，Concat等函数可以作为理解的例子

##### 11.2 使用函数

我们可以使用函数进行

* 处理文本串
* 在数值数据进行算数操作
* 处理日期和时间
* 返回DBMS的特殊信息

等功能

在书p69-74页中有详细的介绍，还有可供查询的表格

#### 第十二章 汇总数据

##### 12.1 聚集函数

在一些情况下，我们需要对数据进行检索但不需要返回数据本身，只需要进行处理，如计算个数，计算行组的和，找出最大值，最小值，平均值...这些情况下，我们仅需要对数据进行汇总，而不需要返回实际的表数据，因此MySQL提供了5个聚集函数，方便我们进行相关的操作

* AVG()     返回某列的平均值
* COUNT()  返回某列的行数
* MAX()      返回某列的最大值
* MIN()        返回某列的最小值
* SUM()       返回某列之和

在书p76-80页中，有对相关函数具体用法的具体实例

##### 12.2 聚集不同值

在实际过程中，我们有时候会对数据有不重复的需要，此时我们使用DISTINCT关键字来解决问题

如：

```sql
SELECT AVG(DISTINCT prod_price) AS avg_priceFROM productsWHERE vend_id=1003;
```

> DISTINCT 在技术上说可以用于MAX(),MIN(),但是这是没有意义的

##### 12.3 组合聚集函数

SELECT语句可以根据需要同时使用多个聚集函数

如：

```sql
SELECT COUNT(*) AS num_items	   MIN(prod_price) AS price_min,	   MAX(prod_price) AS price_max,	   AVG(prod_price) AS price_avgFROM products;
```

#### 第十三章 分组数据

本章将介绍两个新的SELECT子句，`GROUP BY`和`HAVING`子句来方便汇总表内容的子集

##### 13.1 数据分组

如果我们想知道每个供应商返回的产品数目，或者是提供10个产品以上的供应商这样的数据，我们便需要数据分组来实现了

##### 13.2 创建分组

分组是在SELECT语句中的GROUP BY子句中建立的，请看实例：

```sql
SELECT vend_id,COUNT(*) AS num_prodsFROM productsGROUP BY vend_id;
```

表示根据vend_id进行分组统计一个供应商提供的产品数

在p84中，书给出了GROUP BY子句的一些规定

##### 13.3 过滤分组

我们可以使用HAVING操作符来对分组进行一定的过滤，实际上，和WHERE是 十分接近的，不过作用的对象不同，其余包括语法都是一致的，如：

```sql
SELECT cust_id,COUNT(*) AS ordersFROM ordersGROUP BY cust_idHAVING COUNT(*)>=2;
```

此句检索出订单量大于2单的顾客以及其订单数

我们也会遇见同时会使用WHERE和HAVING子句的检索，如：

```sql
SELECT vend_id,COUNT(*) AS num_prodsFROM productsWHERE prod_price>=10GROUP BY vend_idHAVING COUNT(*)>=2;
```

这个句子表示检索出拥有超过2个产品大于10刀的供应商以及产品数

##### 13.4 分组与排序

在进行检索时，不能单独依赖一个句子期望达到相同的效果，同时使用才是正确的方法

##### 13.5 SELECT子句的顺序

SELECT

FROM

WHERE

GROUP BY

HAVING

ORDER BY

LIMIT

#### 第十四章 使用子查询

##### 14.1 子查询

子查询是指嵌套在其他查询中的查询，是对先前我们遇见的简单查询的扩充

##### 14.2 使用子查询进行过滤

本处只给出结果，不给出分析，分析见书p90-92

MySQL是关系型数据库，如果我们想要检索所有订购物品TNT2的客户，我们需要使用下列语句

```sql
SELECT cust_name,cust_contactFROM customersWHERE cust_id IN(SELECT cust_id				FROM orders				WHERE order_num IN(SELECT order_num								  FROM orderitems								  WHERE prod_id='TNT2'));
```

##### 14.3 作为计算字段使用子查询

直接给出实例：

```sql
SELECT cust_name	   cust_state,	   (SELECT COUNT(*)	   FROM orders	   WHERE orders.cust_id=customers.cust_id) AS ordersFROM customersORDER BY cust_name;
```

这个查询叫相关子查询，涉及到了外部表的查询

#### 第十五章 联结表

##### 15.1 联结

###### 15.1.1 关系表

MySQL是一个关系型数据库，在不同的表之间，有时候会有数据重复等情况，所以会同时建立很多个表，在不同的表之间缕清关系，从而达到节省时间，节省空间，减少调整数据的量等效果。

如：

vendors表中有主键vend_id，而在products表中也存储着vend_id这个信息，它将products表与vendors表相关联，从而在products表找到vend_id进一步在vendors表中查找到供应商的信息，在products表中，vend_id这个信息被称为外键

> 能够适应不断增加的工作量而不失败，设计良好的数据库或应用程序称之为可伸缩性好

###### 15.1.2 为什么要使用联结

如果运用不同的表但是想通过一条SELECT语句查询信息，那么我们需要使用联结

> 维护引用完整性：
>
> 在使用联结时，联结需要通过MySQL根据需要建立，它存在于查询的执行中。
>
> 在使用关系表时，我们需要在关系列中插入合法的数据，如在products表中出现了不存在于vendors表的vend_id，那么这个查询就将会失效，我们可以在MySQL中只允许在products表中出现合法值，这就是维护引用完整性的操作，我们可以在第21章中学到使用方法

##### 15.2 创建联结

```sql
SELECT vend_name,prod_name,prod_priceFROM vendors,productsWHERE vendors.vend_id=products.vend_idORDER BY vend_name,prod_name;
```

上例表明了如何使用WHERE子句来进行联结的操作，当且仅当vendors的vend_id与products的vend_id相等时，返回对应行的选择列

###### 15.2.1 WHERE子句的重要性

在进行检索时，MySQL执行的实际上是对两个表的每一行进行匹配，如果不用WHERE进行限定条件，我们将得到的是两个表的笛卡尔积，WHERE所作的是进行一些条件的限定，从而得到想到的数据

> 应该保证所有的联结都有WHERE子句，同时保证过滤条件的正确性，以保证结果的正确

> 叉联结：有时候我们会使用被称作叉联结的返回笛卡尔积的联结类型

###### 15.2.2 内部联结

目前所用的联结我们称为等值联结，它基于两个表之间的相等测试。这种联结也称为内部联结，我们也可以换一种联结语法来达成效果

```sql
SELECT vend_name,prod_name,prod_priceFROM vendors INNER JOIN productsON vendors.vend_id=products.vend_id;
```

此例子中，我们在FROM子句中用`INNERE JOIN`指定了联结类型，用`ON`代替了WHERE子句来限定条件

> 规范中我们应该首先使用INNER JOIN语法，使用明确的联结语法能确保不会忘记使用联结条件，有时也能影响性能

###### 15.2.3 联结多个表

以第14章多个子查询的为例：

```sql
SELECT cust_name,cust_contactFROM customersWHERE cust_id IN(SELECT cust_id				FROM orders				WHERE order_num IN(SELECT order_num								  FROM orderitems								  WHERE prod_id='TNT2'));
```

我们也可以使用联结进行代替

```sql
SELECT cust_name,cust_contactFROM customers,orders,orderitemsWHERE customers.cust_id=orders.cust_idAND orderitems.order_num = orders.order_numAND prod_id='TNT2'
```

这个查询没有使用多重子查询，而是使用了两个联结进行结果的达成，实际上这两种方法都是可以的，没有绝对正确或者是绝对错误的方法，性能会受到数据量，操作类型，是否存在索引等因素影响，如果想挑选出性能最高的方法，我们需要使用不同的机制进行实验

#### 第十六章 创建高级联结

##### 16.1  使用表别名

除了列名和计算字段外，表也可以使用别名，我们可以达成缩短SQL语句和允许在单条SELECT语句中多次使用相同的表的效果，表别名在FROM子句中定义

如：

```sql
SELECT cust_name,cust_contactFROM customers AS c,orders AS o,orderitems AS oiWHERE c.cust_id=o.cust_id	AND oi.order_num=o.order_num	AND prod_id='TNT2';
```

##### 16.2 使用不同类型的联结

###### 16.2.1 自联结

在products表中，你发现某个产品有问题，当你想知道相同生产商的其他产品是否也有问题，你需要两次使用同一个表：

```sql
SELECT prod_id,prod_nameFROM productsWHERE vend_id=(SELECT vend_id			  FROM products			  WHERE prod_id='DTNTR');
```

```sql
SELECT p1.prod_id,p1.prod_nameFROM products AS p1,products AS p2WHERE p1.vend_id=p2.vend_id	AND p2.prod_id='DTNTR';
```

上述两个查询都达成的相同的效果，第二个方法称为自联结查询，为了排除二义性问题，我们使用了表别名，详见p108

###### 16.2.2 自然联结

自然联结是一种自己完成的联结，主要目的是为了排除相同的列，我们通常会使用完全限定列名来达成效果，详见p109

###### 16.2.3 外部联结

有时候我们会需要包含没有关联行的那些行，此时需要用到外部联结

如：

+ 对每个客户下了多少订单进行计数，包括那些没有订单的客户
+ 列出所有产品及订购数量，包括没有人订购的产品
+ 计算平均销售规模，包括那些没有订单的客户

```sql
SELECT customers.cust_id,orders.order_numFROM customers LEFT OUTER JOIN orders	ON customers.cust_id = orders.cust_id
```

这个语句的意思是检索所有的客户及其订单包括那些没有订单的客户

`LEFT`关键字的意思是包含customers表的所有行

如果想要orders的所有行 我们要用`RIGHT`关键字

##### 16.3 使用带聚集函数的联结

我们也可以在带有联结的SELECT语句中使用聚集函数

如

```sql
SELECT customers.cust_name,	   customers.cust_id,	   COUNT(orders.order_num) AS num_ordFROM customers INNER JOIN orders	ON customers.cust_id=orders.cust_id	GROUP BY customers.cust_id
```

```sql
SELECT customers.cust_name,
	   customers.cust_id,
	   COUNT(orders.order_num) AS num_ord
FROM customers LEFT OUTER JOIN orders
	ON customers.cust_id=orders.cust_id
	
GROUP BY customers.cust_id
```



#### 第十七章 组合查询

##### 17.1 组合查询

MySQL支持一句话同时运行多个SELECT语句，并将结果作为单个查询结果集返回，这些组合查询通常称为并（union）或复合查询

当在单个查询中需要从不同的表返回结构类似的数据或者对单个表执行多个查询按单个查询返回数据时，需要用到组合查询。

##### 17.2 创建组合查询

