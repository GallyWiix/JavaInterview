# 数据库分页
#### 优化LIMIT分页
优化分类查询的一个最简单的办法就是尽可能地使用索引覆盖扫描，而不是查询所有的列，然后根据需要做一次关联操作再返回所需的列。对于偏移量很大的时候，这样做的效率会提升非常大
```sql
SELECT film_id,description FROM film ORDER BY title LIMIT 50,5;
```  
如果这个表非常大，则这个语句更好
```sql
SELECT film.film_id,film.description  FROM sakila.film INNER JOIN (  SELECT film_id FROM sakila.film ORDER BY title LIMIT 50,5 ) AS lim USING(film_id);
```

# 聚合函数与多表连接
常用聚合函数的作用  
表与表之间的关联方式：
1. 外连接：
   - 左外连接：LEFT JOIN,返回左表中的所有记录和右表中满足连接条件的记录
   - 右外连接: RIGHT JOIN,返回右表中的所有记录和左表中满足连接条件的记录
2. 内连接：  
内连接通过INNER JOIN来实现，发那会两张表中满足连接条件的数据，不满足条件的不会被查询  

#SQL注入
sql注入的原理是将sql代码伪装到输入参数中，传递到服务器解析并执行的一种攻击手法。对server端发起的请求参数中植入一些sql代码，server端在执行sql操作中，会拼接对应参数，导致执行一些预期之外的操作  

# where和having
where是一个约束声明，使用where约束来自数据库的数据，where是在结果返回之前起作用的，where中不能使用聚合函数  
having是一个过滤声明，是在查询返回结果集以后对查询结果进行的过滤操作，在having中可以使用聚合函数。另一方面，having字句中不能使用除了分组字段和聚合函数之外的其他字段  
从性能的角度来说，HAVING子句中如果使用了分组字段作为过滤条件，应该替换成WHERE子句。因为WHERE可以在执行分组操作和计算聚合函数之前过滤掉不需要的数据，性能会更好。
