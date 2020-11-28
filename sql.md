# 建立表
## 学生表
(学号, 姓名, 出生日期, 性别)
```
create table student(
sid int(11) not null,
sname varchar(20) not null,
birthday date not null,
sex char(4) not null,
primary key(sid)
);
```
## 成绩表
(学号, 课程号, 成绩)
```
create table scores(
sid int(11) not null,
cid int(11) not null,
score decimal not null,
primary key(sid,cid) //联合主键
);
```
## 课程表
(课程号, 课程名称, 教师号)
```
create table class(
cid int(11) not null,
cname varchar(20) not null,
tid int(11) not null,
primary key(cid,tid)
);
```
## 教师表
(教师号, 教师姓名)
```
create table teacher(
tid int(11) not null,
tname varchar(20) not null,
primary key(tid)
);
```
# 查询实战
## 1.查询姓李的学生名单
```
select * from student where sname like '李%';
```
## 2.查询课程编号为2的总成绩
```
//因为where条件不能作用于聚合函数, 所以不能用where cid=2
select sum(score) from scores group by cid having cid=2;
```
## 3.查询各科成绩最高和最低分
```
select cid,max(score),min(score) from scores group by cid;
```
## 4.查询选课总人数
```
//在聚合函数count里面用distinct
select count(distinct sid) from scores;
```
## 5.查询各科选课人数
```
select cid,count(distinct sid) from scores group by cid;
```
## 6.查询男生, 女生人数
```
select sex,count(distinct sid) from student group by sex;
```
## 7.查询平均成绩大于60分的学生的学号和平均成绩
```
select sid,avg(score) from scores group by sid having avg(score)>60;
```
## 8.查询至少选修两门课程的学生学号
```
select sid from scores group by sid having count(cid)>1;
```
## 9.查询相同姓名学生名单并统计人数
```
select sid, count(*) from student group by sname having count(*)>1;
```
## 10.查询不及格课程并按课程号从大到小排列
```
select cid from scores where score<60 group by cid order by cid desc;
```
## 11.统计每门课程的学生选修人数(超过2人的课程才统计), 要求输出课程号和选修人数, 查询结果按人数降序, 若人数相同, 则按课程号升序
```
select cid,count(*) from scores group by cid having count(*)>1 order by count(*) desc,cid asc;
```
## 12.查询两门以上不及格课程的同学的学号以及不及格课程的平均成绩
以下两条都行
```
select sid,avg(score) from scores where score<60 group by sid having count(*)>2;
//用子查询的话, 必须要给派生出来的表起别名, 因此这里的子查询起了别名fuck
select sid,avg(score) from (select sid,score from scores where score <60) as fuck group by sid having count(*)>2;
```
## 13.查询所有成绩小于60分学生的学号, 姓名
```
select sid,sname from student where sid in(select sid from scores group by sid having max(score)<60)as fuck;
```
