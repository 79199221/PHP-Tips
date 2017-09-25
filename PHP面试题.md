---
title: PHP面试题
tags: 新建,模板,小书匠
grammar_cjkRuby: true
---

> 1. 用PHP获取当前时间并打印，打印格式：2006-5-10 22:21:21  
```
date_default_timezone_set('PRC');
echo date('Y-n-d H:i:s');
```

> 2. 字符串转数组、数组转字符串，字符串截取，字符串替换、字符串查找的函数分别是什么？  
```
a. $str = 'www.baidu.com';
	str_split($str, $length);		// 返回以$length为长度值的字符串
	preg_splite($preg, $str);	 // 正则分割
	explode($string, $str);		  // 以$string为界点分割字符串

b. $arr = ['aaa', 'bbb', 'ccc'];
	implode($string, $arr); 	//以$string为连接符连接字符串
	join() 								  // implode()别名
	
c. substr($str, 1, 10);
   mb_substr()
   mb_struct()
  
d. str_replace('www', 'tieba', $str);
   preg_replace('/w{3}/', 'tieba', $str);
   
e. strpos()
	strrpos()
	strstr()
	strchr();
	preg_match()
	preg_match_all()
	

   /* 取路径名，文件名 */
   $str = '/web/a/b/c/index.html';
   $pos = strrpos($str, '/');
   $basename = substr($str, $pos+1)	//文件名
   $dirname = substr($str, 0, $pos)		//路径名
   $basename = basename($str);
   $dirname = diname($str);
```

> 3. 解释一下PHP的类中：protected, public, private, interface, abstract, final, static的含义  
```
a. protected 被保护的
b. public 公有的
c. private 私有的
d. interface 接口：类里面只含有抽象方法的类就可以归为接口
e. abstract 抽象类：含有任意一个抽象方法的类，没有方法体的方法叫抽象方法
f. final 最后的类和方法
g. static 静态方法和属性
```

> 4. 写出下列代码的数据结果  
> $date = '08/26/2003';  
> 2003/08/26  
```
preg_replace('/(\d+)/(\d+)/(\d+)/', $3$1$2, $date);
preg_replace('/([0-9]+)\/([0-9]+)\/([0-9]+)/', '\$(3)\/${1}\/${2}', $date);
explod()       implod();
```

> 5. 从表login中选出name字段包含admin的前10条结果所有信息的sql语句  
```
select * from login where name like '%name%' order by id limit 10;
```

> 6. 解释：左连接、右连接、内连接、索引。  
```
a. right join
b. left join
c. inner join
```
































































