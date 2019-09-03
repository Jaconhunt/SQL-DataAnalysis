1.用一条SQL 语句 查询出每门课都大于80 分的学生姓名

name   kecheng   fenshu
       张三    语文        81
       张三     数学       75
       李四     语文       76
       李四     数学       90
       王五     语文       81
       王五     数学       100
       王五     英语       90

```
create table course_table(
name varchar(10) not null, 
kecheng varchar(10) not null,
fenshu decimal(18,1)
);
insert into course_table values
('张三','语文','81'),
('张三','数学','75'),
('李四','语文','76'),
('李四','数学','90'),
('王五','语文','81'),
('王五','数学','90'),
('王五','英语','100');
```

```
select name from course_table GROUP BY name having min(fenshu) > 80;
#每门课都大于80的学生姓名，所以按学生分组，查询所有分数大于80的
```

2.SQL语句如何查询各个用户最长的连续登陆天数？如图左边是源表User,右边是需要达到的查询结果。

![img](https://upload-images.jianshu.io/upload_images/6328113-c1dd214cf6f700c5.png?imageMogr2/auto-orient/strip|imageView2/2/w/996/format/webp)

```
CREATE TABLE IF NOT EXISTS `load_record` (
  `uid` int(6) unsigned NOT NULL,
  `loadtime` varchar(200) NOT NULL,
  PRIMARY KEY (`uid`,`loadtime`)
) DEFAULT CHARSET=utf8;

INSERT INTO `load_record` (`uid`, `loadtime`) VALUES
  ('201', '2017/1/1'),
  ('201', '2017/1/2'),
  ('202', '2017/1/2'),
  ('202', '2017/1/3'),
  ('203', '2017/1/3'),
  ('201', '2017/1/4'),
  ('202', '2017/1/4'),
  ('201', '2017/1/5'),
  ('202', '2017/1/5'),
  ('201', '2017/1/6'),
  ('203', '2017/1/6'),
  ('203', '2017/1/7');
```

```
Select UID,max(cnt) as cnt
From (
            Select UID,Grp_No,count(*) as cnt
            From (
                    Select UID,LoadTime,(Day(LoadTime)-ROW_NUMBER() OVER (Partition By UID Order By UID,LoadTime)) as Grp_No 
                    From load_record 
                  ) a
            Group By UID,Grp_No
      ) a
Group By UID
```

3.选出每个科目成绩排名前三的学生

```sql
CREATE TABLE IF NOT EXISTS `dataSubject` (
  `id` int(6) unsigned NOT NULL,
  `name` varchar(200) NOT NULL,
  `subject` varchar(200) NOT NULL,
  `score` decimal(10) NOT NULL,
  PRIMARY KEY (`id`)
) DEFAULT CHARSET=utf8;

#插入数据
INSERT INTO dataSubject VALUES (1,'小黄','数学',99), (2,'小黄','语文',89),(3,'小黄','英语',79), (4,'小黄','物理',99), (5,'小黄','化学',98), (6,'小红','数学',89), (7,'小红','语文',99), (8,'小红','英语',79), (9,'小红','物理',89), (10,'小红','化学',69),(11,'小绿','数学',89), (12,'小绿','语文',91), (13,'小绿','英语',92),(14,'小绿','物理',93), (15,'小绿','化学',94), (16,'小黄','数学',100), (17,'小黄','语文',100),(18,'小黄','英语',100), (19,'小黄','物理',60), (20,'小黄','化学',66);
```

```
#row_number  无视相同值
SELECT * FROM(
SELECT *,row_number() over(partition by subject order by score desc) AS 'rank' FROM datasubject) AS t 
WHERE t.rank < 4;

#rank() 遇到相同值跳跃式排名
SELECT * FROM(
SELECT *,rank() over(partition by subject order by score desc) AS 'rank' FROM datasubject) AS t 
WHERE t.rank < 4;

#dense_rank() 遇到相同值连续式排名
SELECT * FROM(
SELECT *,dense_rank() over(partition by subject order by score desc) AS 'rank' FROM datasubject) AS t 
WHERE t.rank < 4;
```

4.各部门员工薪水排名

```
create table employee (empid int ,deptid int ,salary decimal(10,2));
insert into employee values(1,10,5500.00);
insert into employee values(2,10,4500.00);
insert into employee values(3,20,1900.00);
insert into employee values(4,20,4800.00);
insert into employee values(5,40,6500.00);
insert into employee values(6,40,14500.00);
insert into employee values(7,40,44500.00);
insert into employee values(8,50,6500.00);
insert into employee values(9,50,7500.00);
```

```
select *,row_number() over(partition by deptid order by salary desc) as 'rank_rownumber' from `employee`;
```

```
ROW_NUMBER() OVER (PARTITION BY COL1 ORDER BY COL2) 表示根据COL1分组，在分组内部根据 COL2排序，而此函数计算的值就表示每组内部排序后的顺序编号
ROW_NUMBER() OVER (ORDER BY xlh DESC) 是先把xlh列降序，再为降序以后的没条xlh记录返回一个序号。 
```

5.查询一张数据表（tb），基本字段：日期，订单，要求用SQL实现：周次（week），订单总和，日均订单，极大值订单，极小值订单







6.使用SQL实现以下数据表行转列及总分，平均分（数据表：table）

```
create table score_table
(stu_name varchar(10) not NULL,
course varchar(10) not null,
score decimal(10) not null);
```

```
insert into score_table VALUES
('李晓康', '语文', 58),
('张磊', '语文', 68),
('李晓康', '数学',78),
('张磊','数学',88),
('李晓康','英语',98),
('张磊','英语',88);
```

![1567474955136](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1567474955136.png)

```
select *, sum(语文 + 数学 + 英语) as '总分', (语文 + 数学 + 英语)/3 as '平均分' 
from(select stu_name,   
max(case course when '语文' then score else 0 end)   as '语文',
max(case course when '数学' then score else 0 end)   as '数学',
max(case course when '英语' then score else 0 end)   as '英语' 
from score_table as a
group by stu_name) a
group by stu_name;
```

![1567477233715](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1567477233715.png)

7.查询数据表（order），基本字段如下：省，市，店铺名称，订单数。要按”市”排序，需要看到每一个市的订单排名前三名的门店及订单数。





8.用户登陆日志表sys_login，包含user_id，log_time，session_id，plat

1.用sql查询近30天，每天平均登陆用户数量？

2.写sql查询出近30天，连续访问7天以上的用户数量？

```
#分析，每天平均登陆用户，即统计1-30日登陆用户数，累加再平均
select user_id from table 
```



9.交易表结构user_id（用户ID），order_id（订单），pay_time（付款时间），order_amount（金额）。

1.写sql查询过去一个月付款用户量（提示：用户量需去重）最高的3天分别是哪几天？

```
#分析，付款用户量最高的天数，即某天有多个用户付款，完成订单交易
select day(pay_time)
from (
select distinct user_id
)
```

2.写sql查询昨天每个用户最后付款的订单ID及金额？





10.有PV表a（表结构为user_id（用户名），goods_id（商品ID）），点击表b（表结构为user_id（用户名），goods_id（商品ID））两个表，各存放40亿条user_id的goods_id访问记录，在防止数据倾斜的情况下，写一句sql找出a、b两个表共同的user_id与相应的goods_id？

```
select user_id, goods_id from a join b on a.user_id = b.user_id and a.goods_id = b.goods_id;
```

11.对于通过不同渠道拉新进来的用户，经过一段时间许多用户可能流水，而留下来的用户称之为留存用户。分析用户留存是拉新和用户运营的重要指标。假如我们有一张用户访问表：person_visit，记录了所有用户的访问信息，包含字段：用户id：user_id，访问时间：visit_date，访问页面：page_name，访问渠道（android,ios）：plat等

1.请统计近7天每天到访的新用户数。

2.请统计每个访问渠道7天前（D-7）的新用户的3日留存和7日留存率。



12.表group有四个字段，表结构如下：

create table group(

id bigint comment '群号',

name string comment '群名',

class string comment '群类别',

num int comment '群成员数量'

);



数据如下：

1 一起打球 篮球 10

2 来玩球吧 篮球 15

3 滨江一霸 篮球 5

4 足球小将 足球 20

5 绝代双骄 足球 30

6 玩个球啊 乒乓 19

...



PK为id

1、求群数量少于1000的群类别

2、求每个群类别下，群成员数量最多的100个群



12.需要对用户抽样用以分析，用户表结构为user_id（用户ID），reg_time（注册时间），age（年龄）。

1.写一句SQL按user_id尾数随机抽样2000个用户？

2.写一句SQL取出按各年龄段（每10岁一个分段，如：[0,10),[10,20),[20,30)...）分别抽取1%的用户？



13. 通过登录日志表统计用户登录天数（进阶，统计连续登陆天数）


```
userid              login_time   
10001         2018-12-13 14:39:56.300
10001         2018-12-13 14:59:56.100
10002         2018-12-13 16:23:21.200
10003         2018-12-13 23:58:31.500
10001         2018-12-14 11:48:51.200
10003         2018-12-15 13:03:43.400
10004         2018-12-15 15:27:28.100
10004         2018-12-15 23:59:59.100
```

```
select userid, count(distinct day(login_time)) as 登陆天数
from log_day 
group by userid;
```

![1567433734541](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1567433734541.png)


14. 实现行列转换

    ![1567409054541](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1567409054541.png)

![1567410562400](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1567410562400.png)

```
create table course_table
(学号 varchar(10) not null,
课程号 varchar(10) not null,
成绩 decimal(10) 
);

insert into  course_table VALUES
('0001', '0001', '80'),
('0001', '0002', '90'),
('0001', '0003', '99'),
('0001', '0002', '60'),
('0001', '0003', '80'),
('0001', '0001', '80'),
('0001', '0002', '80'),
('0001', '0003', '80');
```

```
select 学号,
max(case 课程号 when '0001' then 成绩 else 0 end) as '课程号0001',
max(case 课程号 when '0002' then 成绩 else 0 end) as '课程号0002',
max(case 课程号 when '0003' then 成绩 else 0 end) as '课程号0003'
from course_table
group by 学号;
```

进阶：

尝试Pandas的行列转换功能，理论上SQL的增删改查Pandas都能做

参考文章：

[1] 数据分析之SQL面试题，https://www.jianshu.com/p/77597eadd3cc

[2] 常见的SQL笔试和面试题，https://zhuanlan.zhihu.com/p/37897135

[3] sqlzoo，https://sqlzoo.net/wiki/SELECT_basics/zh

[4] SQL面经，https://blog.csdn.net/XindiOntheWay/article/details/82697988#_15

[5] sql，https://bbs.csdn.net/topics/392497103

[6] Pandas行列转换，https://blog.csdn.net/zutsoft/article/details/51509124