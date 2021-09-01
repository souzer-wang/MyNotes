`由中文无法存入MySQL的问题开始，梳理MySQL的编码问题`

近日碰到的问题在于：
使用接口创建日志时，如果在字段中存入中文或中文符号，则会报错，无法存入，经过查找资料，确认是编码格式的问题。

> 实际上，在使用 MySQL 数据库的时候，对应的表的字段编码通常默认为 latin1，利用 navicat 或者 Dbeaver 等客户端去查看的话，显示为乱码。通过 jdbc 或者 php 去取也是乱码。
> 
> 但是如果通过mysql -h -u -p 命令登录，查询出的数据中中文是正常显示的。

**当前采用的解决方案为：** 不再使用JPARepository原有的存储函数save，重新加一个save函数实现insert操作，从而修改sql语句，在语句中实现对于字段内容编码的转换

---

以下为相关知识点梳理，参考：[mysql编码问题——charset=utf8你真的弄明白了吗？](https://blog.csdn.net/weixin_41261833/article/details/106223066)

### 一些概念

##### 1. 什么是字符集？

字符集就是 字符和编码 的集合，常用的中文字符集是 gbk，英文字符集是 ASCII，如果有多种字符在同一个字符集里面，通常用UTF8

MySQL 服务器支持多种字符集，在同一台服务器，同一个数据库，甚至同一个表的不同字段，都可以指定使用不同的字符集。 

与之相对的，Oracle 等数据库管理系统在同一个数据库只能使用相同的字符集，相比之下MySQL更显灵活。

在 MySQL 中，字符集的概念和编码方案一个意思，一个字符集就是 一个转换表 和 一个编码方案的组合

##### 2. 什么是 Unicode ？

Unicode 是计算机上使用的字符编码，全称 Universal Code，它是为了解决传统的字符编码方案的局限而产生的

- 为每种语言的每个字符都设定了统一且唯一的二进制编码，从而支持跨平台文本转换处理
- 包含不同的编码方案：UTF-8、UTF-16、UTF-32 其中 UTF 的意思是 Unicode Transformation Format

##### 3. Java 和 MySQL 中编码的对应：

|Java|MySQL|
|:-:|:-:|
|UTF-8|utf8|
|ISO-8859-1|latin1|


MySQL中默认使用的是 latin1，但在建表时可以通过建表语句来指定编码，如：
```sql
create table student(
    sid int primary key aotu_increment,
    sname varchar(20) not null,
    age int
)charset=utf8;
```

---

### 几个命令

```sql
-- 查看数据库支持的所有字符集
mysql> show character set;
-- 查看系统当前状态，里面可以看到部分字符集设置
mysql> status;
-- 查看系统字符集设置，包括所有的字符集设置
mysql> show variable like '%char%';
```

第三条指令查询出的内容如下：
（一二列为查询出的结果，第三列为变量含义）

|变量|字符集|含义|
|:-:|:-:|:-:|
| character_set_client     | latin1| 客户端来源数据使用的字符集|
| character_set_connection | latin1| 连接层字符集|
| character_set_database   | latin1| 当前选中数据库的默认字符集|
| character_set_filesystem | binary| 文件系统的编码格式|
| character_set_results    | latin1| 查询结果字符集|
| character_set_server     | latin1| 默认的内部操作字符集|
| character_set_system     | utf8  | 系统元数据（字段名等）字符集|

各变量具体含义可参考：[关于MySQL中的8个 character_set 变量说明](https://blog.csdn.net/sun8112133/article/details/79921734)

---

### 什么是连接器

上面的查询结果中有一个字段包含 connection，这就是连接器，它的作用是：
连接客户端和服务端，进行字符集的转换。

连接器的工作流程：（1，2 是进，3 是出）
1. 客户端的字符先发给连接器，连接器选择一种编码将其进行转换，临时储存
2. 连接器再次转换成服务器需要的编码，最终存储在服务器中
3. 然后服务器返回的结果通过连接器转换为和客户端一致的字符集，就能在客户端正常显示了

- 如果 client、result 编码一致，connection 和 	server 编码一致，那么编码的转换发生在 连接器 和 客户端、结果 一端，如下图
![[幻灯片3.jpg]]

- 如果 client、result、connection 编码一致，那么编码的转换发生在连接器和server一端，如下：

![[幻灯片4.jpg]]

---

### MySQL中字符集的转换过程

1. MySQL 收到请求后将数据从 character_set_client 转换为character_set_connection
2. 进行内部操作前将数据从 character_set_connection 转换为 内部操作字符集，具体的规则如下：
	1. 使用每个数据字段的 character set 设定值
	2. 如果上述值不存在，使用对应数据表的 default character set 设定值
	3. 如果上述值不存在，使用对应数据库的 default character set 设定值
	4. 如果上述值不存在，使用 character_set_server 设定值
3. 将操作结果从内部操作字符集转换为 character_set_results

---

### Java程序在 MySQL 读写中文的方法

总体思路是一样的，先从 UTF-8 转 latin1，读的时候再转回来。

#### 法一

1. 将 Java 文件设置为 UTF-8 编码
2. 设置 URL 参数 characterEncoding 为 utf8
3. 所有的数据库连接在 之前任何 SQL 语句前，先执行一句：`set names latin1`
4. 取数据时，需要从 ISO-8859-1 转码，示例如下：
```java
new String(xxx.getBytes("ISO-8859-1"), "UTF-8");
```

补充：
- 对于从 JDBC驱动程序发往服务器的所有字符串，在缺省情况下将自动从原生的 Java Unicode 形式转换为客户端字符编码。如果要转成特定编码，则需要设置 characterEncoding
- set names 'latin1' 这个操作会将 `character_set_client`，`character_set_connection`和`character_set_results` 这三个的编码改为 latin1，而这三个字符集的作用在上面的字符集转换过程中已说明。**client的编码原本为 utf-8，在被改为 latin1 后，JDBC 语句会将 SQL 语句进行转码，而此时client和connection编码格式一致，MySQL就不做转换了**

#### 法二

**和上面方法的不同点在于第三点**

1. 将 Java 文件设置为 UTF-8 编码
2. 设置 URL 参数 characterEncoding 为 utf8
3. 将所有 SQL 语句、特别是含有中文的 SQL 语句都必须转码为 “ISO-8859-1”，转换方法上面提到了
4. 取数据时，需要从 ISO-8859-1 转码


补充：
- 正如法一的补充中提到，因为 set names 'latin1' 操作，client 的编码改为了 'latin1'，从而 JDBC 会对 SQL 语句进行转码。所以在替代操作就是手动对 SQL 语句进行编码转换

#### 法三

和法二的思想类似，就是将涉及中文的内容进行编码转换，区别是在
 SQL 语句中利用 convert 函数完成了编码格式的转换。
 
 具体使用方法是：
 
- 传入的名字字段原本为 utf-8 编码，改为 latin1
```sql
insert into students (name, age) values (convert(unhex(hex(convert(name using utf8))) using latin1) , age);
```

- 查询时，将 latin1 编码的内容转为 utf8 编码
```sql
select convert(unhex(hex(convert(name using latin1))) using utf8) as name from students;
```

#### 我的解决方法

我最终采用了方法三
- 最后存储数据时使用了JPA Repository，而 Query 注解中只能执行单条sql语句，所以不能实现法一；
- 如果从修改sql语句编码入手，必须重写save函数，同时SQL 语句作为Query的value值，暂未找到合理的修改语句编码再赋值给value的方式，所以也没用法二。

但法三可以在原 SQL 语句进行修改，实现起来较为简单，因而作为最终方案。

关于数据读取的部分，当前是在php部分中实现的，其中以task.php为例，通过执行`set session character_set_results=utf8`语句，在读取内容前完成了编码修改。

### 为什么 MySQL 不能存入中文

首先 MySQL 的默认字符集 latin1 是不支持中文的，这样从客户端发送中文过去，连接器发现mysql服务器使用的是 latin1 ，就会进行转换，但这样的转化会有问题，就像大鱼过小网，对于字符集来说，就会丢失字节。而丢失字节后存入的值，肯定是错误的。

由于 MySQL 的检测比较严格，存入的值既然是错误的，索性不让存入，这就是为什么插入中文的时候会报错。

当然，索性把服务器的字符集设置为 utf-8 也可解决问题，但一般不这么做。



