# SQL语句

## 基本查询

`SELECT column_name,column_name FROM table_name`;

`SELECT * FROM table_name`;

`SELECT DISTINCT column_name FROM table_name`;

> DISTINCT 关键词用于返回唯一不同的值。

`SELECT * FROM table_name WHERE column_name =value`;

> WHERE 子句用于提取那些满足指定条件的记录。

`SELECT * FROM table_name WHERE country='USA' OR id=1`

> 如果第一个条件和第二个条件都成立，则 AND 运算符显示一条记录。
>
> 如果第一个条件和第二个条件中只要有一个成立，则 OR 运算符显示一条记录。

`SELECT column_name FROM table_name ORDER BY column_name DESC`;

> ORDER BY 关键字默认按照升序对记录进行排序。如果需要按照降序对记录进行排序，您可以使用 DESC 关键字。

* **插入**

`INSERT INTO table_name VALUES (value1,value2,value3,...)`;

`INSERT INTO table_name (column1,column2,column3,...) VALUES (value1,value2,value3,...)`;

> 插入的第二种形式需要指定列名及被插入的值：

* **更新**

`UPDATE table_name SET column1=value1,column2=value2 WHERE some_column=some_value`;

* **删除**

`DELETE FROM table_name WHERE some_column=some_value`;

## 高级语句

` SELECT * FROM table_name LIMIT 2,3`; 

> LIMIT 语句来选取指定的条数数据，2,3表示从第二行开始往后取3条数据

`SELECT * FROM table_name WHERE name LIKE 'G%'`;

> 上面的SQL语句选取 name 以字母 "G" 开始的所有客户：

`select * from table_city where Address not like '%市'`

> 上面的SQL语句选取不以“市”结尾的城市

***SQL简单通配符***

| 通配符 | 描述                |
| ------ | ------------------- |
| %      | 替代 0 个或多个字符 |
| _      | 替代一个字符        |

`SELECT * FROM table_city WHERE name IN ('北京市','天津市');`

>  IN 操作符允许您在 WHERE 子句中规定多个值。 

`SELECT * FROM table_person WHERE age **BETWEEN** 18 **AND** 30;`

>  上面的SQL语句选取年龄介于 1 到 20 之间的人的数据

`SELECT column_name(s) FROM table1 INNER JOIN table2 ON table1.column_name=table2.column_name`

`SELECT column_name(s) FROM table1 LEFT JOIN table2 ON table1.column_name=table2.column_name`

>  LEFT JOIN 关键字从左表（table1）返回所有的行，即使右表（table2）中没有匹配。如果右表中没有匹配，则结果为 NULL。 

`SELECT column_name(s) FROM table1 RIGHT JOIN table2 ON table1.column_name=table2.column_name`

>  RIGHT JOIN 关键字从右表（table2）返回所有的行，即使左表（table1）中没有匹配。如果左表中没有匹配，则结果为 NULL。 

`SELECT column_name(s) FROM table1`
**UNION**
`SELECT column_name(s) FROM table2;`

> UNION 操作符用于合并两个或多个 SELECT 语句的结果集。
>
> 请注意，UNION 内部的每个 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每个 SELECT 语句中的列的顺序必须相同。

`SELECT column_name, aggregate_function(column_name)`
`FROM table_name`
`WHERE column_name operator value`
**GROUP BY** column_name;

> GROUP BY 语句用于结合聚合函数，根据一个或多个列对结果集进行分组。

`SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name`
**HAVING** aggregate_function(column_name) operator value; 

> 在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与聚合函数一起使用。
>
> HAVING 子句可以让我们筛选分组后的各组数据。

## SQL函数

SELECT **AVG**(column_name) FROM table_name

SELECT **COUNT**(*) FROM table_name







































