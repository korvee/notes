# 变量声明

## 声明时直接赋值

```sql
declare @seqno int = 1
declare @seqno as int = 1
```

## 使用select赋值

```sql
declare @name nvarchar(20)
--1、select结果为空，@name值不变而不是赋值为null
--2、查询结果为多条时也不会报错，而是取最后一个值赋值；也可以增加top 1 指定第一个
select @name = name from sys.tables
```

# 数据类型

## 日期和时间

### 三种时间类型

date不带时分秒，datetime到毫秒，smalldatetime到秒，更精确的可以使用datetime2指定秒后精确到的位数，转为字符串显示如下

| date       | datetime                | smalldatetime       | datetime2(5)              |
| ---------- | ----------------------- | ------------------- | ------------------------- |
| 2024-02-03 | 2025-04-10 18:54:28.257 | 2024-02-03 00:00:00 | 2025-04-10 18:56:39.75667 |

### 日期转字符串

三个最常用方式

| convert(varchar,getdate(),112) | convert(varchar,getdate(),23) | convert(varchar,getdate(),20) |
| ------------------------------ | ----------------------------- | ----------------------------- |
| 20250410                       | 2025-04-10                    | 2025-04-10 19:12:40           |

### 常用日期函数

```sql
select month(getdate()),--月份
eomonth(getdate()),--月底日期
dateadd(month,1,getdate()),--一个月以后
dateadd(day,-1,getdate())--昨天

--这里发生了自动类型转换，需要日期类型参数时可以传符合日期格式的字符
select datediff(day,'20240204','20250301'),
datediff(month,'20240204','20250301')

--2月只有29天，1月20 30 31加一个月，都返回2月29
select dateadd(month,1,'20240129')
,dateadd(month,1,'20240130'),dateadd(month,1,'20240131')
```

## 数字

1. decimal和numeric是同义词，可以互换使用

2. decimal[(p[,s])]可选的参数p和s，p是精度，表示十进制数字的总位数上限（小数点两端位数之和），最大38，默认18,s是小数位数，p-s就是整数位最大位数，注意整数位要留足，不够会溢出报错，小数位不够则会四舍五入

## 字符串

1. varchar和nvarchar不同，varchar1代表一个字节，而中文要两个字节，而nvarchar代表字符，所有字符统一占一个（存储中文和字母的时候这样说是对的，其他类型的字符可能不满足，实际还是要看字符存储的内容占几个字节）

2. 增加N'前缀标识unicode常量，对应nvarchar类型，可以正确存储日文等非拉丁字符

```sql
select N'var1','var1'
```

# 常用函数

```sql

--choose函数根据第一个int参数的值，选择返回后面对应顺序的字符串
select CHOOSE(MONTH(getdate()),'Winter','Winter', 'Spring','Spring','Spring','Summer','Summer',   
                          'Summer','Autumn','Autumn','Autumn','Winter')
--iif实现条件满足和不满足时分别返回的值，isnull只在为空时返回替换
select iif(2 is null,2,1) --len返回字符数，datalength返回字节数
select len('数据库'),datalength('数据库')
--newid返回guid唯一标识
declare @id1 nvarchar(200) = newid()
select @id1
--返回上个sql语句影响的行数
select @@rowcount

--自增主键，可指定初始值和步长
create table dbo.tab(
id int identity(1,1) primary key,
name nvarchar(200)
)


```

# 集合操作

1. union 合并后去重

2. union all 只合并不去重

3. except 返回前结果集中独有的记录

4. intersect 返回两个集合中共有的记录

# merge

```sql
--merge用于增量更新数据，根据匹配结果指定操作
merge target_tab as target
using source_tab as source
on target.id = source.id
when matched then
when not matched by target then
when not matched by source then
```

# with

```sql
--公共表表达式，是一种临时的命名结果集，可以在查询中多次引用
with table_temp(id,name,name1)
as
(select id,name,name1 from table1)
```

# 三种排序方法

```sql

select id,score,

--并列的名次一样，且排名序号连续
dense_rank() over (order by score desc) as denserank,
--并列的名次一样，排名序号不连续
rank() over (order by score desc) as rank,
--排名序号连续，并列的也分先后
row_number() over (order by score) as rownumber
from dbo.rank_test

```



# 异常处理

```sql
--error_line和error_message函数获取错误信息
begin try
    insert into dbo.inventory values (1,1)
end try
begin catch
    print(concat('第',error_line(),'行执行错误，错误信息：',error_message())
```

# curror

```sql
--cursor的经典用法，读取符合条件的记录到cursor，逐行将值放到变量中处理
declare @id int,@name nvarchar(200)
declare cursor1 cursor
for select id,name from dbo.inventory where id  < 10
open cursor1
fetch next from cursor1 into @id,@name
while @@fetch_status = 0
	begin
	fetch next from cursor1 into @id,@name
	end
close cursor1
deallocate cursor1
```

# 事务

```sql
begin transaction [事务名称]
commit transaction [事务名称]
rollback transaction [事务名称]
```
