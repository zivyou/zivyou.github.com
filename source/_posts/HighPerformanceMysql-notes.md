---
title: HighPerformanceMysql notes
date: 2020-06-04 23:42:58
tags:
---
# 「完全的范式化和反范式化schema都是实验室里才有的东西」

> 范式化的优点:
  - 范式化的更新操作通常比反范式化要快
  - 当数据较好地范式化时，就只有很少或者没有常负数据，所以值需要修改更少的数据
  - 范式化的表通常更小，可以更好地放在内存里，所以执行更快
  - 很少有多余的数据意味着检索列表数据时更少需要DISTINCT或者GROUP语句

> 范式化的缺点

  - 通常范式化需要大量的表关联，稍微复杂一些的查询语句在符合范式的schema上都可能需要至少一次关联

> 反范式化的优点: 
    - 可以很好地避免关联
    - 可以设计更好的索引策略

# 「影子表」
```
drop table if exists my_summary_new, my_summary_old;
create table my_summary_new like my_summary;
rename table my_summary to my_summary_old, my_summary_new to my_summary;
```

# 数据类型选型:

  - 使用小而简单的合适数据类型，除非真实数据模型中有确切的需要，否则应该尽可能的避免使用NULL值
  - 尽量使用相同的数据类型存储相似霍相关的值，尤其是要在关联条件中使用的列
  - 注意可变长字符串，其在临时表和排序时可能导致悲观的按最大长度分配内存
  - 小心使用ENUM和SET，不能滥用。最好避免使用BIT
   
# B-Tree索引

> -  一般讨论索引的时候，都是指B树索引
> -  InnoDB使用的是B+树
> -  B-Tree对索引列是顺序组织存储的，所以很适合查找范围数据
> -  B-Tree适用于一下类型的查找: 全键值、键值范围、键值前缀(指适用于最左前缀匹配)
> -  索引树中的所有节点都是有序的，所以也适用于order by排序操作
   
# 哈希索引

> - 哈希索引指包含哈希指和行指针，而不存储字段值
> - 
> - 哈希索引数据并不是按照索引值顺序存储的，所以无法用于排序
> - 
> - 哈希索引也不支持部分索引列匹配查找
> - 
> - 哈希索引值支持等值比较查询，包括=, IN(), <=>，不支持任何范围查询
> - 
> - 如果哈希冲突很多，一些索引的维护操作的代价高
> - 
> - 可以在业务层手动为一些字段设置哈希函数，实现"伪哈希索引"


# 空间数据索引(R-Tree)

> MyISAM的特性，用于存储地理信息数据(3维GIS)，提供了一系列内置函数

# 全文索引

> 待后文详解

# 前缀索引
```
alter table sakila.city_demo add key (city(7))
```
> 无法做group by和order by

# 聚簇索引

> 聚簇索引描述的是一种索引的存储结构。InnoDB中的聚簇索引是在同一个结构中保存B树的索引和数据行，所以所以结构和数据存放在同一个文件中。

> 优势:

  - 减少磁盘IO
  - 数据访问更快
> 缺点:

  - 如果数据全部放在了内存中，则聚簇索引就没有什么意义了
  - 
  - 数据插入的速度严重依赖于插入顺序
  - 
  - 更新聚簇索引列的代价搞
  - 
  - 页分裂问题
  - 
# 覆盖索引

> 案例
```
select * from products where actor='SEAN CARRY' and title like '%APOLLO%';
```
# 慢查询

> 典型的负面案例

  - 查询不需要的attributes
  - 表关联时返回全部attributes
  - select *
  - where中包含in 子查询: select * from tableA where tableA.fid in (select id from tableB where name='xxx');
> mysql使用where的3种方式(以下从好到坏列出)
  - 在索引中用where，过滤不匹配的行。在存储引擎中完成
  - 使用索引覆盖扫描(explain extra: using index)，在mysql服务层完成，无需回表查询
  - 从数据表中返回数据，然后过滤不满足条件的行，explain extra: using where，在mysql服务层完成，需要回表查询
> 重构查询

  - 复杂查询拆分成几步，可在业务代码中完成
  - 利用循环，采用类似于分页的方式分批查询
  - 分解关联查询
> 关联查询的原理

  - 所有的关联动作都是先生成一个临时表，然后通过嵌套查询的方式遍历。简单来说，如果A, B, C3表关联查询，3表分别有x, y, z行数据，则关联的复杂度为x * y * z;

> 关联查询优化

  - 所有的查询，mysql都会预先生成一个查询计划。查询计划是有很多个的，mysql会自己选择查询计划中看起来代价最小的那个

  - 多表关联查询的查询计划，通常都是一棵左侧深度优先的树
  - mysql的关联查询优化器会通过调整关联顺序尝试自己选择合适的查询计划: 当n个表关联时，会去尝试n!(n的阶乘)种排列方式。显然，n不能太大

> 排序优化

  - 数据量小的时候，mysql会在内存排序，数据量大了，就是在磁盘排序了。但在explain extra中看到的都是filesort

  - mysql使用「快排」在内存中进行排序; 当在磁盘时，使用快排分段排序，再merge回磁盘

> 查询的执行

  - mysql的执行计划是一个数据结构
  - 通过调用存储引擎的「handler API」完成。
  - 查询优化器的一些提示关键字(hint): 用来指示查询优化器如何按照我们定制化的方案执行。类似于gcc的内置宏

# 一些常见的优化招式

> 优化count()

  - count()既可以统计行数，也可以统计某个列值的数量。统计列值的时候mysql会计算列值不为空的数量。如果mysql确认count()括号里的内容不可能为空，此时统计的其实就是行数
  - 所以如果要统计行数，请直接使用count(*)
  - count(*)会扫描所有行，这是不可避免的，如果有必要，可以新增一张汇总表来解决
> 优化关联查询

  - 尽量让on或者using的字段有索引
  - 确保group by和order by里的表达式只涉及到一个表中的列
> 优化子查询

  - 尽量用关联查询代替

> 优化group by和distinct

  - 使用索引来优化，这是最有效的办法

  - A left join B ，最好只对A中的列做group by

> 优化limit + offset + order by的分页方法

  - 尽可能使用索引覆盖扫描。利用表关联，先通过覆盖索引找出id， 再做一次关联查询查询行的其他信息
```
select film_id, description from sakila.film order by title limit to 50, 5
建议改成:
select film.film_id, film.description
from sakila.film
	inner join (
  	select film_id from sakila.film
    order by title limit 50, 5
  ) as lim using(film_id)
```
  - 利用书签记录一下上次翻页的位置，然后利用>, <, between 加上索引的方式，可以减少无用的扫描次数

> 使用用户自定义sql变量来优化

  - 自定义变量在整个查询连接过程中都存在
  - 使用了自定义变量的查询，无法使用查询缓存

> 优化select for update
表格式
```
create table unsent_emails (id int not null primary key auto_increament, status enum('unsent', 'claimed', 'sent')
                  owner int unsigned not null default 0, ts timestamp, key (owner, status, ts)
                )
```
常规的用法:
```
begin;
select id from unsent_emails
  where owner=0 and status='unsent'
  limit 10 for update;
-- result: 123, 456, 789
update unsent_emails
  set status='claimed', owner=connection_id(),
  where id in(123, 456, 789)
```
改进的用法:
```
set autocommit=1;
commit;
update unsent_emails
  set status='claimed', owner=connection_id()
  where owner=0 and status='unsent'
  limit 10;
set autocommit=0;
select id from unsent_emails
  where owner=connection_id() and status='claimed';
-- result 123,456,789 
```

______

# 案例1
翻页问题:  mysql可以使用limit m,n的方式支持翻页。但是在实现上，mysql查找第m条记录需要将前m条记录都扫一遍。因此当m比较大的时候，效率低。
解决方法:
1. 借助自增ID，定位m的值，较少扫描的行数。例如，limit m,n 的时候，我们可以改成 where id>m order by id ASC limit n; 效果提升明显
2. 借助索引。具体想法是先找出limit m,n的id(id自带索引)，然后inner join整条记录:
```
select * from TableA inner join (select id from TableA limit m,n) as TableB using(id);
```

