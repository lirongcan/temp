```
查询男生, 女生人数
select 性别, count(distinct 学号) from student group by 性别;

查询平均成绩大于60分的学生的学号和平均成绩
select 学号, avg(成绩) as '平均成绩' from score group by 学号 having avg(成绩)>60;

查询至少选修两门课程的学生学号
select 学号, count(课程号) as '选修门数' from score group by 学号 having count(课程号)>=2;

查询同名同姓(相同姓名)学生名单并统计人数
select 姓名, count(*)as '人数" from student group by 姓名 having count(*)>1;

查询不及格课程并按课程号从大到小排列
select 课程号, 成绩 from score where 成绩<60 group by 课程号 order by 课程号 desc;

统计每门课程的学生选修人数(超过2人的课程才统计)
要求输出课程号和选修人数, 查询结果按人数降序排序, 若人数相同, 按课程号升序排序
select 课程号, count(*)as '选修人数' from score group by 课程号 having count(*)>2 order by count(*)desc, 课程号asc;

查询两门以上不合格课程的同学的学号, 以及不及格课程的平均成绩
select 学号, avg(成绩) from (select 学号, 成绩 from score where 成绩<60) as 不及格名单 group by 学号 having count(成绩)>2;

查询所有成绩小于60分学生的学号, 姓名
select 学号, 姓名 from student where 学号 in (select 学号 from score group by 学号 having max(成绩)<60);
```
