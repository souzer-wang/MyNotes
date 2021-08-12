# explain 的用法

<br>

## 字段含义

参考链接：[SQL优化-explain的用法（实例解析）_啊荻～的博客-CSDN博客](https://blog.csdn.net/qq_16268979/article/details/106652565)

<br>

### id

1. id 值相同的情况下，从上往下顺序执行
2. id 值不同的情况下，id 值越大越优先查询

<br>

### select_type

- PRIMARY: 包含子查询 SQL 中的主查询（最外层）
- SUBQUERY: 包含子查询SQL中的 子查询（非最外层）  
- SIMPLE: 简单查询（不包含子查询、union）  
- DERIVED: 衍生查询（使用到了临时表）
在子查询中，如果有 table1 union table2，则 table1 就是derived, table2 就是 union
- UNION：如上
- UNION RESULT: 告知开发人员，哪些表之间存在 union 查询

<br>

### table

实际在哪张查询，这里的表可能是实际存在的表，也可能是临时表(如衍生表（derived）、union 等)

<br>

### type

常见的几种，性能按从高到低排序： system > const > eq_ref > ref > range > index > all

其中： system，const 只是理想情况，实际能达到 ref > range

1. **system**：只有一条数据的系统表或衍生表只有一条数据的主查询
2. **const**：仅仅能查到一条数据的 SQL，并且用于 Primary key 或 union 索引
3. **eq_ref**: 唯一性索引：对于每个索引键的查询，返回匹配**唯一行数据**（**有且只有1个，不能多、不能0**）
4. **ref**: 非唯一性索引，对于每个索引键的查询，**返回匹配的所有行（可以是0，多个）**
5. **range**: 检索指定范围的行，where 后面是一个范围查询（between,in,>,<,>=等）
6. **index**: 查询全部索引中的数据
7. **all**: 查询全部表中的数据

<br>

### possible_key

可能用到的索引

<br>

### key

实际用到的索引

<br>

### key_len

索引的长度

作用：用来判断复合索引是否完全被使用

<br>

### ref

作用：指明当前表所参照的字段

若为const 说明是常量  如 age = 23

<br>

### rows

被索引优化查询的数据个数

<br>

### Extra

含以下几种情况：

1. **using filesort**

性能消耗大，需要额外的一次排序/查询。

**常见于 order by 语句中**，因为排序之前必然是先查询

例子：
```sql
create table test02(
 a1 char(3),
 a2 char(3),
 a3 char(3),
 index idx_a1(a1),
 index idx_a2(a2),
 index idx_a3(a3)
);
```

```sql
explain select * from test02 where a1 = '' order by a1;
//using where
//给a1排序之前先查询了a1，所以不用using filesort

explain select * from test02 where a1 = '' order by a2;
//using where,using filesort
//给a2排序之前只查询了a1，而a2没有查询，所以额外对a2的一次查询然后才能完成a2的排序，即用到using filesort
```

小结： 对于单索引，排序和查找同一个字段，就不会using filesort； 如果排序和查找不是同一个字段，则会出现using filesort。

复合索引： 不能跨列（最佳左前缀）

```sql
drop index idx_a1 on test02;
drop index idx_a2 on test02;
drop index idx_a3 on test02;

alter table test02 add index idx_a1_a2_a3(a1,a2,a3);
explain select * from test02 where a1='' order by a3;//using filesort
explain select * from test02 where a2='' order by a3;//using filesort
explain select * from test02 where a1='' order by a2;//没有using filesort
```

小结：order by 和 where 按照复合索引的顺序使用，不要跨列或无序使用


2. **using temporary **

性能损耗大，用到了临时表

额外多使用一张表，出现在 group by 语句中


3. **using index**

性能提升，也叫索引覆盖，不读取原文件，只从索引文件中获取数据（不需要回表查询）

只要用到的列全部在索引中，就是索引覆盖using index


4. **using where**

需要回表查询


5. **impossible where**

where 子句永远为 false