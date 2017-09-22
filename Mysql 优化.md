---
title: Mysql 优化
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---


# 一、表的优化与列类型选择
> 表的优化   

1. 定长与变长相分离：  
```
如 id int 占4个字节， char(4) 占4个字符长度，也是定长，time
即每一单元值占的字节是固定的。
核心且常用字段，宜建定长，放在一张表。

而varchar, text, blob这种变长字段，适合单放一张表，用主键与核心表关联起来。
```

2. 常字段和不常用字段要分离。  
```
需要结合网站具体的业务来分析，分析字段的查询场景。查询频度低的字段，单拆出来。
```

3. 在一对多，需要关联统计的字段上，添加冗余字段

---

> 列选择原则：
1. 字段类类型优先级：整型 > date, time > enum, char >varchar > blob, text
```
int：定长，没有国家、地区之分，没有字符集的差异
比如：tinyint 1, 2, 3, 4, 5 <——> char(1) a,b,c,d,e
从空间上看，都 是占一个字节，但是order by 排序，前者快
原因：后者需要考虑字符集与校对集（就是排序规则）
time 字长，运算快，节省空间，烤炉时区，写sql时不方便 where> '2017-11-11';
enum 能起到约束值的目的，内部用整型来存储，但与char联查时，内部要经历串与值的转化。
char 定长，考虑字符集和（排序）校对集
varchar 不定长 要考虑字符集的转换与排序时的校对集，速度慢。
text/blob 无法使用内存临时表（排序等操作只能在磁盘上进行）

性别：（以UTF8为例）
char(1), 3个字长字节
enum(‘男', '女');内部转成数字来存，多了一个转换过程
tinyint(), //0 1 2 定长 1个字节
```

2. 够用就行，不要慷慨  
```
原因：在的字段浪费内存，影响速度。
以年龄为例 tinyint unsigned not null, 可以存储255，足够，用int浪费了3个字节
以varchar(100), varchar(300)存储的内容相同，但在表联查时，varchar(300)要花更多内存
```

3. 尽量避免使用NULL  
```
原因：NULL不利于索引，要用特殊的字节来标注。
在磁盘上占据的空间其实更大（mysql5.7已对NULL做了改进，但查询仍是不便）
```

# 索引优化策略
> 索引类型

1. B-tree 索引
```

```

2. B-tree 常见误区
a. 在where添加常用列上都加上索引
```
例：where cat_id = 3 and price > 100; 
误：cat_id上，和price上都加上索引
错：只能用上cat_id或price索引，因为是独立索引，同时只能用上1个
```


#    
1. limit 及翻页优化  
```
limit offset,N, 当offset非常大时，效率极低，
原因是mysql并不是跳过offset行，然后单取N行，
而是取offset+N行，返回放弃前offset行，返回N行。
效率较低，当offset越大时，效率越低。
```
优化办法：
> 1. 从业务上去解决  
```
办法：不允许翻过100页
以百度为例，一般翻页到70页左右
```
> 2. 不用offset,用条件查询   
```
例：
/*前提条件，数据不进行物理删除，只进行逻辑删除*/
select id, name from user limit 10000000, 10;
优化为：
select id, name from user where id>10000000, 10
```
> 3. 查索引优化  
```
select * from user
inner join 
( select id from user limit 10000000, 10) as tmp
on user.id = tmp.id
```