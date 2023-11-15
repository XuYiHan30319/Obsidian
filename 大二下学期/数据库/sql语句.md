这部分太杂了,自己看书吧![[-10f4f1651dcb4466.jpg]]
# select语句的大体形式
![[Pasted image 20230316085306.png]]
having和where的区别:where先执行,having后执行,where是过滤行,having过滤组
转义:后面跟上ESCAPE'\\'
```sql
select * from sc
where sname like '123\%' escape '\'
```
## 聚组函数
count(\*) 统计所有行数.count(sc)统计这一列非空记录数
count(distinct sc)
一般出现在select和having后面
注意,聚组函数不能出现在where的后面,因为这样不能起到过滤行的作用,比如我们这样
![[Pasted image 20230327195756.png]]
除了count,聚组函数还有avg,max,min,sum,注意,聚组函数在计算的时候自动忽略null值
![[Pasted image 20230327201423.png]]
## 相关子查询和不想关子查询
相关子查询指子查询需用到主查询的某些列
不相关子查询没有主查询也能执行子查询
```sql
select 
from A
where exists(相关子查询,一般使用select * from B where 列=A列)
```
呃呃,这里提示一下,<font color="#ff0000">exist没啥用(可以用多表查询实现),any和all也没啥用(可以用max.min函数实现)</font>(老师说的)
exists只看返回结果空不空,只要不空就是true,所以可以使用\*来代表
关键字any,x>=any(...)表示比最大的还大
x<>all(..)相当于not in
注意:any和all只能返回一列的值
如果子查询为空,all返回true,any返回false;
![[Pasted image 20230323083723.png]]
## 换个名字
在查询中,我们随时能给列或者表取个别名,在自连接查询的时候必须取别名
![[Pasted image 20230327200538.png]]
如果在多表查询的时候有两个或者以上的列名字重复了,必须指明他是哪个表的列
## 去重
我们可以使用distinct关键字来对查询的结果去重
![[Pasted image 20230327200820.png]]
## group by
group by能对表按照给定的进行分组,然后使用聚组函数的时候会对每一个分组分别进行运算,group by 后面只能跟列名不是别名 
![[Pasted image 20230327201804.png]]
在group by的时候我们可以添加多个字段,不过一般用不上,而且效果也不咋地,两个字段分组就是要同时考虑两个列，两个列中都是一模一样的数据则分在同一个组中，就比如 黑色的狗狗是一个组、白色狗狗是一个组.select后面的列名如果没有聚组函数可以和group by的不一致
![[Pasted image 20230327201946.png]]
## having
having就是用来对分组之后的结果进行进一步筛选,比如我们想选择选课门数大于1的人,的学号,我们不能直接在where中使用count(\*)>1来进行筛选(根据老师说很多人喜欢这样做),而是应该先group by后再在having中执行![[Pasted image 20230327202141.png]]
## order by
order by用于对选出的数据表进行排序,默认排序为升序
![[Pasted image 20230327202331.png]]
我们可以使用order by sno desc来实现降序![[Pasted image 20230327202352.png]]
## 多表查询
我们有时候查询只使用一张表做不到,我们就需要使用多表查询的方式来实现
比如我们想要查询有科目考了90分且至少考了2门的学生的名字,我们就需要多表查询
```sql
select student.sname,student.sno,avg(grade) as avg
from student,sc
where student.sno = sc.sno and sc.sno in(
	select sno
	from sc
	where grade>90
)
group by student.sno
having count(*)>1;
```
![[Pasted image 20230327203209.png]]
## 杂项

如果我们想要查询等于Null的元祖,我们不可以使用=来查询,而是应该使用is null
```sql
select * 
from sc
where grade is null;
```
![[Pasted image 20230327203553.png]]
对于表达式匹配,%号表是多个字符,\_表示一个字符,并且我们要使用like
![[Pasted image 20230327203718.png]]
![[Pasted image 20230327203755.png]]
![[Pasted image 20230327203837.png]]
如果名字里面带有_或者%需要使用,我们必须使用转义字符
```sql
select sno from sc
where cn like "db\_design" escape '\';
```
选出考试的人数
```sql
select count(distinct sno) from sc;
```

## UNION 并集
union就是合并两个select的结果,方法如下
```sql
(select city from branch)
union
(select city from property);
```
要求两个select的结果的类型相互匹配否则报错
## INTERSECT 交集
同上,就是把union改为intersect
```sql
select b.city from branch b,proerty p
where b.city = p.city;
等价于
(select b.city from bracch) 
union 
(select city from property);
```
## minus差集
等价于
```sql
select distinct city from branch b
where not exist(
select * from property p
where b.city = p.city
);
```

## 自连接
如果查询涉及到了一张表的不同行数据,就要进行自连接
比如想要查找即选了课程1又选了课程2,就要使用多表查询
```sql
select sno
from t1,t2
where t1.sno=t2.sno and t1.cno=1 and t2.cno = 2;

select * from table a inner join table b on a.id = b.id;
```
## 外连接
```sql
select b.*,p.*
from branch left join
property p
on p.city = branch.city
where p.city = "landon";
```
注意,mysql好像不支持全连接
自连接




-----
# DDL
## 建表
data definition language
```sql
建表
create table sells(
	bar char(10) unique,
	beer varchar(20) references beers(name),
	price int,
	primary key (bar,beer),
	foreign key(beer) references beers(name)
);
```
有些数据库会自动在主键上创建索引,主键不能为null不能重复,但是unique可以为null
## 加强外检约束
有如下几种加强外键约束
1. cascade 级联删除,父亲删除,自己也删除
2. set null 父亲删除,自己置空
3. set defaullt父亲删除,自己设置为默认,注意默认值需要有外键
4. no action 取消本次操作

```sql
create table sc(
	sno char(8),
	foreign key(sno)
		references student(sno)
		on delete set null
		on update cascade
);
```
## 基于类型的约束
这个是列级的约束,作用是用户自定义约束
```create table sells(
	sno char(8) check (sno in (select sno from student)),
	grade int check (grade <=100 and grade >=0),
	
);
```
同时,我们也需要注意删除的时机,check只作用在新的值插入的时候,check (price <5)只检查插入的新值
check (beer in (select name from beers));只检查插入的新值,并且在外键删除的时候不会检查,这是他和外键的区别
## 基于元组的约束
说白了就是把一列后面的check拿出来单独成一行
## alter table
修改表结构,可以add,drop,设置默认值,修改一列的信息,重新命名一个列
```sql
alter table sc modify position drop default;

alter table sc add grade int(8) default 0;

alter table sc drop column grade;

drop table sc(restrict/cascade);
```
在删除表的时候,可以使用restrict(保证没有外键)或者其他的语句来删除
## 索引
用来加速查询
```sql
创建唯一索引
create unique index pro on property(propertyNo);

create index uopname on emp(upper(ename));

drop index indexname;
```
## 视图
```sql
create view ss(sno,grade)
as select sno,grade from sc where grade >80;
```
视图是一个虚表,所以不能使用alter,我们应该直接删掉然后重建.
DBMS维护数据字典,数据字典是电脑自己维护的,用户只能看数据字典但是不能改,数据字典都是大写的
schema下有所有的数据库建表信息,用户和schema是等价的,所有这个用户建立的表都是属于这个schema的.可以是索引,表结构,触发器,视图
如果我们的视图使用了聚组函数或者从不止一张表中获取并且视图中没有包含主键,那么我们就不能从这个视图插入
![[Pasted image 20230330090817.png]]
约束条件大概有:not null(列级的约束),unique(允许为空),primary key(既要非空又要唯一),foreign key(外键,参照完整性),check(用户自定义完整性)
作用于多个属性,必须是表意级,可以一个可以多个可以是表也可以是列级
### with check option
如我们有一个视图,他的定义如下
```sql
create view sc(sno,grade)
select sno,grade from sc where grade >80;
```
如果我们像这个表中插入小于成绩小于80也是可以的,但是如果我们这样
```sql
create view sc(sno,grade)
select sno,grade from sc where grade >80
with check option;
```
他就会自己检验对不对,如果不符合要求就拒绝
## 外检约束
外键必须参考主键或者唯一键
外键约束作用于列直接跟references 表名,如果列一级,需要写 foreign key(..) references ...
我们还可以跟 on delete cascade(级联删除)/set null(对应字段改为空,注意不是主键才行)/no action(子记录存在,那么主键不能删除)/set default,当删除主表的时候会执行后面的
## 类型
varchar和char,varchar是可变长度的,char定长
Date,要写成2位的月份,比如'2023-09-03'
## 主键跟唯一的差别
唯一可以为空,主键不能为空,可以多个唯一,系统会自动建立一个跟主键同名的index,唯一可能不会建立
# 视图限制
- 不能聚组函数嵌套
- 用了聚组函数一定要重命名(mysql不用)
- 用了group by的视图不能和别的视图做链接
- 视图查询是先消解然后和select语句进行融合新形成一个select语句
# 视图更新
- 只有一张表上不用聚组函数的时候可以增删改
- 一张男生视图想要插个女生是可以的,但是建施图的时候使用了with check option的时候就不能插入,会自动检测是否符合where里的条件

什么时候能够添加?
- 要包含主键和非空健(不违反完整性约束),而且只来自一张表并且没有使用聚组函数
- 没有使用distinct
- 一个列名只能出现一次
- 没有嵌套查询
- 没有group by

with check option
行进来视图或者离开视图叫做行迁移(因为表里面修改了),withcheckoption能够限制行迁移,将来对视图的一切操作都必须满足where条件
# 物化视图
某个视图经常被访问,那么这个视图的查询等操作都会受到影响
所以使用物化视图,但是容易存放过时的数据,所以困难在于怎么维护实时性
所以经常用于精度不那么高的数据,周期性重构物化视图
