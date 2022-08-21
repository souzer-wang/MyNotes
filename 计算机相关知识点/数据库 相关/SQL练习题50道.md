[原文链接](https://blog.csdn.net/paul0127/article/details/82529216)

## 常见题目

1. 用一条SQL语句，查询出每门课都大于八十分的学生姓名

```sql
select name from table group by name having min(grade) > 80
```

2. 现有学生表格如下：

|自动编号|学号|姓名|课程编号|课程名称|分数|
|:-:|:-:|:-:|:-:|:-:|:-:|
|1|2005001|张三|0001|数学|69|
|2|2005002|李四|0001|数学|89|
|3|2005001|张三|0001|数学|69|

现在需要删除除了自动编号不同，其它信息都相同的学生冗余信息：
```sql
delete from table where 自动编号 not in (
	select min(自动编号)
	from table
	group by 学号,姓名,课程编号,课程名称,分数
)
```

3. 一个叫team的表，只有一个字段name，有四条记录（a,b,c,d），分表代表四个球队，现在需要给出所有的比赛组合：

```sql
select a.name,b.name
from team a, team b
where a.name < b.name
```

4. 如何把数据表

|year |month |amount|
|:-:|:-:|:-:|
|1991|1|1.1|
|1991|2|1.2|
|1991|3|1.3|
|1991|4|1.4|
|1992|1|2.1|
|1992|2|2.2|
|1992|3|2.3|
|1992|4|2.4|

查询成

|year| m1| m2| m3| m4|  
|:-:|:-:|:-:|:-:|:-:|
|1991| 1.1| 1.2| 1.3| 1.4|  
|1992| 2.1| 2.2| 2.3| 2.4|

```sql
select year, 
	(select amount from table m where month=1 and m.year = table.year) as m1,
	(select amount from table m where month=2 and m.year = table.year) as m2,
	(select amount from table m where month=3 and m.year = table.year) as m3,
	(select amount from table m where month=4 and m.year = table.year) as m4
from table group by year
```




有四张表：
- 学生表
- 课程表
- 教师表
- 成绩表

具体信息如下：

### 学生表 Student

```sql
create table Student(SID varchar(10), Sname varchar(10), Sage datetime, Ssex varchar(10));
insert into Student values('01' , '赵雷' , '1990-01-01' , '男');
insert into Student values('02' , '钱电' , '1990-12-21' , '男');
insert into Student values('03' , '孙风' , '1990-12-20' , '男');
insert into Student values('04' , '李云' , '1990-12-06' , '男');
insert into Student values('05' , '周梅' , '1991-12-01' , '女');
insert into Student values('06' , '吴兰' , '1992-01-01' , '女');
insert into Student values('07' , '郑竹' , '1989-01-01' , '女');
insert into Student values('09' , '张三' , '2017-12-20' , '女');
insert into Student values('10' , '李四' , '2017-12-25' , '女');
insert into Student values('11' , '李四' , '2012-06-06' , '女');
insert into Student values('12' , '赵六' , '2013-06-13' , '女');
insert into Student values('13' , '孙七' , '2014-06-01' , '女');
```

### 科目表 Course

```sql
create table Course(CId varchar(10),Cname nvarchar(10),TId varchar(10));
insert into Course values('01' , '语文' , '02');
insert into Course values('02' , '数学' , '01');
insert into Course values('03' , '英语' , '03');
```

### 教师表 Teacher

```sql
create table Teacher(TId varchar(10),Tname varchar(10));
insert into Teacher values('01' , '张三');
insert into Teacher values('02' , '李四');
insert into Teacher values('03' , '王五');
```

### 成绩表 SC

```sql
create table SC(SId varchar(10),CId varchar(10),score decimal(18,1));
insert into SC values('01' , '01' , 80);
insert into SC values('01' , '02' , 90);
insert into SC values('01' , '03' , 99);
insert into SC values('02' , '01' , 70);
insert into SC values('02' , '02' , 60);
insert into SC values('02' , '03' , 80);
insert into SC values('03' , '01' , 80);
insert into SC values('03' , '02' , 80);
insert into SC values('03' , '03' , 80);
insert into SC values('04' , '01' , 50);
insert into SC values('04' , '02' , 30);
insert into SC values('04' , '03' , 20);
insert into SC values('05' , '01' , 76);
insert into SC values('05' , '02' , 87);
insert into SC values('06' , '01' , 31);
insert into SC values('06' , '03' , 34);
insert into SC values('07' , '02' , 89);
insert into SC values('07' , '03' , 98);
```


### 练习题：
1.  查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数

1.1 查询同时存在" 01 "课程和" 02 "课程的情况

1.2 查询存在" 01 "课程但可能不存在" 02 "课程的情况(不存在时显示为 null )

1.3 查询不存在" 01 "课程但存在" 02 "课程的情况

2.  查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩
    
3.  查询在 SC 表存在成绩的学生信息
    
4.  查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩(没成绩的显示为 null )
    

4.1 查有成绩的学生信息

5.  查询「李」姓老师的数量
    
6.  查询学过「张三」老师授课的同学的信息
    
7.  查询没有学全所有课程的同学的信息
    
8.  查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息
    
9.  查询和" 01 "号的同学学习的课程 完全相同的其他同学的信息
    
10.  查询没学过"张三"老师讲授的任一门课程的学生姓名
    
11.  查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩
    
12.  检索" 01 "课程分数小于 60，按分数降序排列的学生信息
    
13.  按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩
    
14.  查询各科成绩最高分、最低分和平均分：
    

以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率

及格为>=60，中等为：70-80，优良为：80-90，优秀为：>=90

要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列

15.  按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺

15.1 按各科成绩进行排序，并显示排名， Score 重复时合并名次

16.  查询学生的总成绩，并进行排名，总分重复时保留名次空缺

16.1 查询学生的总成绩，并进行排名，总分重复时不保留名次空缺

17.  统计各科成绩各分数段人数：课程编号，课程名称，\[100-85\]，\[85-70\]，\[70-60\]，\[60-0\] 及所占百分比
    
18.  查询各科成绩前三名的记录
    
19.  查询每门课程被选修的学生数
    
20.  查询出只选修两门课程的学生学号和姓名
    
21.  查询男生、女生人数
    
22.  查询名字中含有「风」字的学生信息
    
23.  查询同名同性学生名单，并统计同名人数
    
24.  查询 1990 年出生的学生名单
    
25.  查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列
    
26.  查询平均成绩大于等于 85 的所有学生的学号、姓名和平均成绩
    
27.  查询课程名称为「数学」，且分数低于 60 的学生姓名和分数
    
28.  查询所有学生的课程及分数情况（存在学生没成绩，没选课的情况）
    
29.  查询任何一门课程成绩在 70 分以上的姓名、课程名称和分数
    
30.  查询不及格的课程
    
31.  查询课程编号为 01 且课程成绩在 80 分以上的学生的学号和姓名
    
32.  求每门课程的学生人数
    
33.  成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩
    
34.  成绩有重复的情况下，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩
    
35.  查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩
    
36.  查询每门功成绩最好的前两名
    
37.  统计每门课程的学生选修人数（超过 5 人的课程才统计）。
    
38.  检索至少选修两门课程的学生学号
    
39.  查询选修了全部课程的学生信息
    
40.  查询各学生的年龄，只按年份来算
    
41.  按照出生日期来算，当前月日 < 出生年月的月日则，年龄减一
    
42.  查询本周过生日的学生
    
43.  查询下周过生日的学生
    
44.  查询本月过生日的学生
    
45.  查询下月过生日的学生
    

#### 1. 查询01课程比02课程成绩高的学生的信息和课程分数

因为需要全部的学生信息，所以可以先在sc表中得到符合条件的SID，然后与student表进行 left / right join

```sql
select s.*, a.score as score_01, b.score as score_02
from student s,
     (select sid, score from sc where cid=01) a,
     (select sid, score from sc where cid=02) b
where a.sid = b.sid and a.score > b.score and s.sid = a.sid
```

#### 2. 查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩

```sql
select s.sid, sname, avg(score) as avg_score
	from student as s, sc
	where s.sid = sc.sid
	group by s.sid
	having avg_score > 60
```

#### 3. 查询在 SC 表存在成绩的学生信息

```sql
select * from student where sid in (select sid from sc where score is not null)
```  

#### 4. 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩(没成绩的显示为 null )

这道题得用到left join或者right join，不能用where连接，因为题目说了要求有显示为null的，where是inner join，不会出现null，在这道题里会查不出第08号学生。

```sql
select s.sid, s.sname, count(cid) as 选课总数, sum(score) as 总成绩
	from student as s left join sc
	on s.sid = sc.sid
	group by s.sid
```

#### 4.1 查有成绩的学生信息

```sql
select s.sid, s.sname, count(*) as 选课总数, sum(score) as 总成绩,
    sum(case when cid = 01 then score else null end) as score_01,
    sum(case when cid = 02 then score else null end) as score_02,
    sum(case when cid = 03 then score else null end) as score_03
from student as s, sc
where s.sid = sc.sid
group by s.sid
```

#### 5. 查询「李」姓老师的数量

```sql
select count(tname) from teacher where tname like '李%'
```

#### 6. 查询学过「张三」老师授课的同学的信息

```sql
select * from student where sid in (
    select sid from sc, course, teacher
    where sc.cid = course.cid
     and course.tid = teacher.tid
     and tname = '张三'
)
```

####  7. 查询没有学全所有课程的同学的信息

```sql
select * from student where sid in (select sid from sc group by sid having count(cid) < 3)
```

#### 8. 查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息

```sql
select * from Student where Sid in(
    select distinct Sid from SC where Cid in(
        select Cid from SC where Sid='01'
    )
)
```

#### 9. 查询和" 01 "号的同学学习的课程完全相同的其他同学的信息

```sql
select * from Student
where Sid in(
    select Sid from SC
    where Cid in (select Cid from SC where Sid = '01') and Sid <>'01'
    group by Sid
    having COUNT(Cid)>=3
)
```

#### 10. 查询没学过"张三"老师讲授的任一门课程的学生姓名

```sql
select sname from student
where sname not in (
    select s.sname
    from student as s, course as c, teacher as t, sc
    where s.sid = sc.sid
        and sc.cid = c.cid
        and c.tid = t.tid
        and t.tname = '张三'
)
```


#### 11. 查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩

```sql
select s.sid, s.sname, avg(score)
	from student as s, sc
	where s.sid = sc.sid and score<60
	group by s.sid
	having count(score)>=2
```

#### 12. 检索“01”课程分数小于60，按分数降序排列的学生信息

```sql
select s.* ,score
	from student as s, sc
	where cid = 01
 	 and score < 60
 	 and s.sid=sc.sid
	order by score desc
```

#### 13. 按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩

```sql
select sid,
    sum(case when cid=01 then score else null end) as score_01,
    sum(case when cid=02 then score else null end) as score_02,
    sum(case when cid=03 then score else null end) as score_03,
    avg(score)
from sc group by sid
order by avg(score) desc
```

#### 14. 查询各科成绩最高分、最低分和平均分，以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率(及格为>=60，中等为：70-80，优良为：80-90，优秀为：>=90）。

```sql
select c.cid as 课程号, c.cname as 课程名称, count(*) as 选修人数,
    max(score) as 最高分, min(score) as 最低分, avg(score) as 平均分,
    sum(case when score >= 60 then 1 else 0 end)/count(*) as 及格率,
    sum(case when score >= 70 and score < 80 then 1 else 0 end)/count(*) as 中等率,
    sum(case when score >= 80 and score < 90 then 1 else 0 end)/count(*) as 优良率,
    sum(case when score >= 90 then 1 else 0 end)/count(*) as 优秀率
from sc, course c
where c.cid = sc.cid
group by c.cid
order by count(*) desc, c.cid asc
```

#### 15. 按平均成绩进行排序，显示总排名和各科排名，Score 重复时保留名次空缺

原题目是按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺。但是我没看明白什么意思，各科成绩如何排序？语文分数和数学分数有可比性吗？作者的写法是select \*,RANK()over(order by score desc)排名 from SC，把所有的成绩都放到一块儿排序了，这没有意义，不可比。于是我修改了一下题目。

```sql
select s.*, rank_01, rank_02, rank_03, rank_total
from student s
	left join (select sid, rank() over(partition by cid order by score desc) as rank_01 from sc where cid=01) A on s.sid=A.sid
	left join (select sid, rank() over(partition by cid order by score desc) as rank_02 from sc where cid=02) B on s.sid=B.sid
	left join (select sid, rank() over(partition by cid order by score desc) as rank_03 from sc where cid=03) C on s.sid=C.sid
	left join (select sid, rank() over(order by avg(score) desc) as rank_total from sc group by sid) D on s.sid=D.sid
order by rank_total asc
```

#### 15.1 按平均成绩进行排序，显示总排名和各科排名，Score 重复时合并名次

同样修改了一下题目。15题和15.1题的指向很明确了，就是rank()和dense\_rank()的区别，也就是两个并列第一名之后的那个人是第三名(rank)还是第二名(dense\_rank)的区别。

```sql
select s.*, rank_01, rank_02, rank_03, rank_total
from student s
	left join (select sid, dense_rank() over(partition by cid order by score desc) as rank_01 from sc where cid=01) A on s.sid=A.sid
	left join (select sid, dense_rank() over(partition by cid order by score desc) as rank_02 from sc where cid=02) B on s.sid=B.sid
	left join (select sid, dense_rank() over(partition by cid order by score desc) as rank_03 from sc where cid=03) C on s.sid=C.sid
	left join (select sid, dense_rank() over(order by avg(score) desc) as rank_total from sc group by sid) D on s.sid=D.sid
order by rank_total asc
```

#### 17. 统计各科成绩各分数段人数：课程编号，课程名称，\[100-85\]，\[85-70\]，\[70-60\]，\[60-0\] 及所占百分比

```sql
select c.cid as 课程编号, c.cname as 课程名称, A.*
from course as c,
(select cid,
    sum(case when score >= 85 then 1 else 0 end)/count(*) as 100_85,
    sum(case when score >= 70 and score < 85 then 1 else 0 end)/count(*) as 85_70,
    sum(case when score >= 60 and score < 70 then 1 else 0 end)/count(*) as 70_60,
    sum(case when score < 60 then 1 else 0 end)/count(*) as 60_0
from sc group by cid) as A
where c.cid = A.cid
```

#### 18. 查询各科成绩前三名的记录

```sql
select * from (
	select *, rank() 
	over(partition by cid order by score desc) 
	as graderank from sc
	) A 
	where A.graderank <= 3
```

#### 20. 查询出只选修两门课程的学生学号和姓名

```sql
select s.sid, s.sname, count(cid)
	from student s, sc
	where s.sid = sc.sid
	group by s.sid
	having count(cid)=2
```

#### 22. 查询名字中含有「风」字的学生信息

```sql
select * from student where sname like '%风%'
```

#### 24. 查询 1990 年出生的学生名单

```sql
select * from student where year(sage) = 1990
```

#### 33. 成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩

```sql
select s.*, max(score)
from student s, teacher t, course c, sc
where s.sid = sc.sid
    and sc.cid = c.cid
    and c.tid = t.tid
    and t.tname = '张三'
```

#### 34. 成绩有重复的情况下，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩

```sql
select * from (
    select *, DENSE_RANK() over (order by score desc) A
    from SC
    where Cid = (select Cid from Course where Tid=(select Tid from Teacher where Tname='张三'))
) B
where B.A=1
```

#### 40. 查询各学生的年龄，只按年份来算

```sql
select sname, year(now())-year(sage) as age from student
```

#### 41. 按照出生日期来算，当前月日 < 出生年月的月日则，年龄减一

```sql
select sname, timestampdiff(year, sage, now()) as age from student
```

#### 42. 查询本周过生日的学生

```sql
select \* from student where week(now()) = week(sage)
```

#### 43. 查询下周过生日的学生

```sql
select \* from student where (week(now())+1) = week(sage)
```

#### 44. 查询本月过生日的学生

```sql
select \* from student where month(now()) = month(sage)
```

#### 45. 查询下月过生日的学生

```sql
select \* from student where (month(now())+1) = month(sage)
```