# 1-7章

MySQL ROOT 密码：abc123

```sql
#启动mysql
net start mysql80
#停止mysql
net stop mysql80
#登录mysql
mysql -uroot -pabc123
```

指令

```sql
#选择数据库
use mydatabase;
#显示数据库
show mydatabase;
#显示数据库中的可用表
show tables;

#检索单个列
select prod_name from products;
#检索多个列
select prod_id, prod_name, prod_price from products;
#检索所有列
select * from products
#检索不同行
select vend_id from products;
#DISTINCT 关键字， 指示MySQL只返回不同的值
select DINSTINCT vend_id from products;
#limit 5指示MYSQL返回不多于5行
select prod_name from products limit 5;
#LIMIT 5, 5指示MySQL返回从行5开始的5行。第一个数为开始位置，第二个数为要检索的行数
select prod_name from products limit 5,5;
#使用完全限定的列名
select products.prod_name from products;
#使用完全限定的表名
select products.prod_name from crashcourse.products;

```

# 第6章 过滤数据

- 使用WHERE子句,指定搜索条件进行过滤

```
select prod_name, prod_price from poducts where prod_price = 2.50;
```

- - 这条语句从products表中检索两个列，但不返回所有行，只返回prod_price值为2.50的行。

- WHERE子句操作符

| 操作符  | 说明               |
| ------- | ------------------ |
| =       | 等于               |
| <>      | 不等于             |
| !=      | 不等于             |
| <       | 小于               |
| <=      | 小于等于           |
| >       | 大于               |
| >=      | 大于等于           |
| BETWEEN | 在指定的两个值之间 |

- 检索单个值

```
select  prod_name, prod_price from products where prod_name = 'fuses';
```

- 何时使用引号 如果仔细观察上述WHERE子句中使用的条件，会看到有的值括在单引号内（如前面使用的'fuses'），而有的值未括起来。单引号用来限定字符串。如果将值与串类型的列进行比较，则需要限定引号。用来与数值列进行比较的值不用引号。



- 范围值检查

```
select prod_name, prod_price from products where prod_price between 5 and 10;
```

这两个值必须用AND关键字分隔。BETWEEN匹配范围中所有的值，包括指定的开始值和结束值。

- 控制检查 NULL

```
select cust_id from customers where cust_email IS NULL;
```

# 第7章 数据过滤

- 组合where子句

```
select prod_id, prod_price, prod_name from products where vend_id = 1003 and 
prod_price <= 10;

select prod_id, prod_price, prod_name from products where vend_id = 1002 or 
prod_price = 1003;
```

- 计算次序

SQL中，AND比OR的操作符优先级更高，使用时注意加括号。

```
select prod_name, prod_price from products
where (vend_id = 1002 or vend_id = 1003) and prod_price >= 10;
```

- IN操作符

IN取合法值的由逗号分号的清单，全都括在圆括号中。

```
SELECT prod_name, prod_id FROM products
WHERE vend_id IN (1002, 1003)
ORDER BY prod_name;
```

- 为什么要使用IN操作符？其优点具体如下。

- - 在使用长的合法选项清单时，IN操作符的语法更清楚且更直观。
  - 在使用IN时，计算的次序更容易管理（因为使用的操作符更少）。
  - IN操作符一般比OR操作符清单执行更快。
  -  IN的最大优点是可以包含其他SELECT语句，使得能够更动态地建立WHERE子句。第14章将对此进行详细介绍。

- IN WHERE子句中用来指定要匹配值的清单的关键字，功能与OR相当。



- NOT操作符

否定它之后所跟的任何条件

```
SELECT prod_name, prod_price
FROM products
WHERE vend_id NOT IN (1002, 1003)
ORDER BY prod_name;
```

- - 为什么使用NOT？对于简单的WHERE子句，使用NOT确实没有什么优势。但在更复杂的子句中，NOT是非常有用的。例如，在与IN操作符联合使用时，NOT使找出与条件列表不匹配的行非常简单
