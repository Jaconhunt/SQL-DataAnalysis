```sql
create table Student1(
Sno varchar(10) NOT NULL PRIMARY KEY,
Sname varchar(10),
Sage datetime,
Ssex char(2)
)ENGINE = INNODB DEFAULT CHARSET = utf8;
CREATE TABLE teacher1(
tno VARCHAR(8) NOT NULL PRIMARY KEY,
tname VARCHAR(10)
)ENGINE = INNODB DEFAULT CHARSET = utf8;
CREATE TABLE course1(
cno VARCHAR(8) NOT NULL PRIMARY KEY,
cname VARCHAR(20),
tno VARCHAR(8),
FOREIGN KEY(tno) REFERENCES teacher1(tno)
)ENGINE = INNODB DEFAULT CHARSET = utf8;
create table Score1
(Sno varchar(10),
Cno varchar(8),
score decimal(18,1),
FOREIGN KEY(sno) REFERENCES student1(sno),
FOREIGN KEY(cno) REFERENCES course1(cno)
)ENGINE = INNODB DEFAULT CHARSET = utf8;
```

```
insert into Student1 values('01' , '丁一' , '1990-01-01' , '男');
insert into Student1 values('02' , '包二' , '1990-12-21' , '男');
insert into Student1 values('03' , '张三' , '1990-05-20' , '男');
insert into Student1 values('04' , '李四' , '1990-08-06' , '男');
insert into Student1 values('05' , '王五' , '1991-12-01' , '女');
insert into Student1 values('06' , '赵六' , '1992-03-01' , '女');
insert into Student1 values('07' , '田七' , '1989-07-01' , '女');
insert into Student1 values('08' , '汪八' , '1990-01-20' , '女');

insert into Teacher1 values('01' , '赵磊');
insert into Teacher1 values('02' , '钱淼');
insert into Teacher1 values('03' , '孙犇');

insert into Course1 values('01' , '语文' , '02');
insert into Course1 values('02' , '数学' , '01');
insert into Course1 values('03' , '英语' , '03');

insert into SCORE1 values('01' , '01' , 80);
insert into SCORE1 values('01' , '02' , 90);
insert into SCORE1 values('01' , '03' , 99);
insert into SCORE1 values('02' , '01' , 70);
insert into SCORE1 values('02' , '02' , 60);
insert into SCORE1 values('02' , '03' , 80);
insert into SCORE1 values('03' , '01' , 80);
insert into SCORE1 values('03' , '02' , 80);
insert into SCORE1 values('03' , '03' , 80);
insert into SCORE1 values('04' , '01' , 50);
insert into SCORE1 values('04' , '02' , 30);
insert into SCORE1 values('04' , '03' , 20);
insert into SCORE1 values('05' , '01' , 76);
insert into SCORE1 values('05' , '02' , 87);
insert into SCORE1 values('06' , '01' , 31);
insert into SCORE1 values('06' , '03' , 34);
insert into SCORE1 values('07' , '02' , 89);
insert into SCORE1 values('07' , '03' , 98);
```



1. 查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数
2. 查询同时存在" 01 "课程和" 02 "课程的情况
3. 查询存在" 01 "课程但可能不存在" 02 "课程的情况(不存在时显示为 null )
4. 查询不存在" 01 "课程但存在" 02 "课程的情况
5. 查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩
6. 查询在 SC 表存在成绩的学生信息
7. 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩(没成绩的显示为 null )
8. 查有成绩的学生信息
9. 查询「赵」姓老师的数量 
10. 查询学过「钱淼」老师授课的同学的信息 
11. 查询没有学全所有课程的同学的信息 
12. 查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息 
13. 查询和" 01 "号的同学学习的课程完全相同的其他同学的信息 
14. 查询没学过"张三"老师讲授的任一门课程的学生姓名 
15. 查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩 
16. 检索" 01 "课程分数小于 60，按分数降序排列的学生信息
17. 按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩
18. 查询各科成绩最高分、最低分和平均分：
19. 以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率，及格为>=60，中等为：70-80，优良为：80-90，优秀为：>=90，要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列
20. 按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺
21. 按各科成绩进行排序，并显示排名， Score 重复时合并名次
22. 查询学生的总成绩，并进行排名，总分重复时保留名次空缺
23. 查询学生的总成绩，并进行排名，总分重复时不保留名次空缺
24. 统计各科成绩各分数段人数：课程编号，课程名称，[100-85]，[85-70]，[70-60]，[60-0] 及所占百分比
25. 查询各科成绩前三名的记录
26. 查询每门课程被选修的学生数 
27. 查询出只选修两门课程的学生学号和姓名 
28. 查询男生、女生人数
29. 查询名字中含有「风」字的学生信息
30. 查询同名同性学生名单，并统计同名人数
31. 查询 1990 年出生的学生名单
32. 查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列
33. 查询平均成绩大于等于 85 的所有学生的学号、姓名和平均成绩 
34. 查询课程名称为「数学」，且分数低于 60 的学生姓名和分数 
35. 查询所有学生的课程及分数情况（存在学生没成绩，没选课的情况）
36. 查询任何一门课程成绩在 70 分以上的姓名、课程名称和分数
37. 查询不及格的课程
38. 查询课程编号为 01 且课程成绩在 80 分以上的学生的学号和姓名
39. 求每门课程的学生人数 
40. 成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩
41. 成绩有重复的情况下，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩
42. 查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩 
43. 查询每门功成绩最好的前两名
44. 统计每门课程的学生选修人数（超过 5 人的课程才统计）。
45. 检索至少选修两门课程的学生学号 
46. 查询选修了全部课程的学生信息
47. 查询各学生的年龄，只按年份来算 
48. 按照出生日期来算，当前月日 < 出生年月的月日则，年龄减一
49. 查询本周过生日的学生
50. 查询下周过生日的学生
51. 查询本月过生日的学生
52. 查询下月过生日的学生