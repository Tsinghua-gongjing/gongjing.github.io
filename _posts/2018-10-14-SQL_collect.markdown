---
layout: post
category: "linux"
title:  "SQL常见问题"
tags: [linux, SQL]
---

- TOC
{:toc}

### `left join`时的`on`和`where`

* 参看[SQL中的Join和Where的区别](https://developer.aliyun.com/article/376565)
* left join + on + where
	* 在查询多张表的时候，会生成一张中间表
	* on：在生成临时表的时候使用的条件，不管此条件是否为真，都会返回左表的记录（这是left join的特性所决定的）
	* where：在得到临时表的结果之后，对临时表进行过滤的条件，条件不满足的记录就被过滤掉了
	* 具体参见上述链接页面的例子

---

### 相同的id根据其他列的值选择最大或者最小的

* 参考[SQL 多条数据中取最大值？](https://bbs.csdn.net/topics/392295893)

```sql
# 表格

ID     Uname  Price   BuyDate
1      张三        180     2017-12-1
2      张三        280     2017-12-7
3      李四        480     2017-12-10
4      李四        280     2017-12-11
5      王武        280     2017-12-1
6      王武        880     2017-12-11
7      王武        380     2017-12-15

# 想要
ID     Uname  Price   BuyDate
2      张三        280     2017-12-7
3      李四        480     2017-12-10
6      王武        880     2017-12-11

# 语句
SELECT * FROM (
    SELECT *,ROW_NUMBER()OVER(PARTITION BY Uname ORDER BY Price DESC ) AS rn FROM #t
) AS t WHERE t.rn=1

# 使用降序或者升序配合n=1，可以选择最小或者最大的
# 配合类似n<=5，可以选择最小或者最大的5个
```

---

### 查询语句执行顺序

需要遵循以下顺序：

|子句|说明|是否必须使用|
|---|---|---|
|SELECT|要返回的列或者表达式|是|
|FROM|从中检索数据的表|仅在从表中选择数据时使用|
|WHERE|行级过滤|否|
|GROUP BY|分组说明|仅在按组计算聚集时使用|
|HAVING|组级过滤|否|
|ORDER BY|输出排序顺序|否|
|LIMIT|要检索的行数|否|

---

### `LIMIT`的用法

* 用法：`select * from tableName limit i,n`
* `i`：查询结果的索引值，默认从n开始
* `n`：查询结果返回的数量

```sql
--检索前10行数据，显示1-10条数据
select * from Customer LIMIT 10;

--检索从第2行开始，累加10条id记录，共显示id为2....11
select * from Customer LIMIT 1,10;

--检索从第6行开始向前加10条数据，共显示id为6,7....15
select * from Customer limit 5,10;

--检索从第7行开始向前加10条记录，显示id为7,8...16
select * from Customer limit 6,10;
```

---

### `OFFSET`的用法

* 用法：`OFFSET n`
* `n`：表示跳过n个记录
* 注意：当 limit和offset组合使用的时候，limit后面只能有一个参数，表示要取的的数量，offset表示要跳过的数量。

```sql
-- 跳过第一个记录
select * from article OFFSET 1

-- 跳过第一个记录，提取接下来的3个记录
-- 当LIMIT和OFFSET联合使用的时候，limit后面只能有一个参数
select * from article LIMIT 3 OFFSET 1

-- 跳过第一个记录，从index=1开始，提取接下来的3个记录
select* from article LIMIT 1,3
```