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

```mysql
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

```mysql
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

```mysql
select  prod_name, prod_price from products where prod_name = 'fuses';
```

- 何时使用引号 如果仔细观察上述WHERE子句中使用的条件，会看到有的值括在单引号内（如前面使用的'fuses'），而有的值未括起来。单引号用来限定字符串。如果将值与串类型的列进行比较，则需要限定引号。用来与数值列进行比较的值不用引号。



- 范围值检查

```mysql
select prod_name, prod_price from products where prod_price between 5 and 10;
```

这两个值必须用AND关键字分隔。BETWEEN匹配范围中所有的值，包括指定的开始值和结束值。

- 控制检查 NULL

```mysql
select cust_id from customers where cust_email IS NULL;
```

# 第7章 数据过滤

- 组合where子句

```mysql
select prod_id, prod_price, prod_name from products where vend_id = 1003 and 
prod_price <= 10;

select prod_id, prod_price, prod_name from products where vend_id = 1002 or 
prod_price = 1003;
```

- 计算次序

SQL中，AND比OR的操作符优先级更高，使用时注意加括号。

```mysql
select prod_name, prod_price from products
where (vend_id = 1002 or vend_id = 1003) and prod_price >= 10;
```

- IN操作符

IN取合法值的由逗号分号的清单，全都括在圆括号中。

```mysql
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

```mysql
SELECT prod_name, prod_price
FROM products
WHERE vend_id NOT IN (1002, 1003)
ORDER BY prod_name;
```

- 为什么使用NOT？对于简单的WHERE子句，使用NOT确实没有什么优势。但在更复杂的子句中，NOT是非常有用的。例如，在与IN操作符联合使用时，NOT使找出与条件列表不匹配的行非常简单

# 第8章 用通配符进行过滤

- **谓词**    操作符在它作为谓词时，不是操作符。

## 1.百分号（%）通配符

- % 表任何字符出现任意次数。

  ```mysql
  SELECT prod_id, prod_name
  From products
  WHERE prod_name LIKE 'jet%';
  ```

  - 区分大小写

- ```mysql
  SELECT prod_id, prod_name
  From products
  WHERE prod_name LIKE '%anvil%';
  
  SELECT prod_id, prod_name
  From products
  WHERE prod_name LIKE 's%e'; #找s起头e结尾的产品
  ```

  - % 不能匹配NULL值作为产品名的行。

## 2.下划线（_） 通配符

- 只能匹配单个字符

- ```mysql
  SELECT prod_id, prod_name
  From products
  WHERE prod_name LIKE '_ton anvil'; 
  ```

#  第九章 用正则表达式进行搜索

- 正则表达式是用来匹配文本的特殊的串（字符集合）

```mysql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000'
ORDER BY prod_name;

SELECT prod_name
FROM products
WHERE prod_name REGEXP '.000' #.是正则表达式语言中一个特殊的字符。它表示匹配任意一个字符，因此，1000和2000都匹配且返回。
ORDER BY prod_name;
/*如果 regexp 需要匹配整个字段, 最好使用^$ 
	重要区别
like 整个字段匹配表达式成功才返回
regexp 部分字符匹配表达式成功即可返回
*/

#进行OR匹配
SELECT prod_name
FROM products
where prod_name regexp '1000|2000'
order by prod_name;

select prod_name
from prodcuts
where prod_name regexp '[123] Ton'
order by prod_name;

select prod_name
from prodcuts
where prod_name regexp '[^123] Ton'
order by prod_name;

#匹配范围
select prod_name from products where prod_name regexp '[1-5] Ton' order by prod_name;
#匹配特殊字符
select vend_name from vendors where vend_name regexp '\\.' order by vend_name;
#匹配字符类

```

<img src="D:\C++编程学习资料\我的笔记\images\image-20230412210417574.png" alt="image-20230412210417574" style="zoom:50%;" />

<img src="D:\C++编程学习资料\我的笔记\images\image-20230412211209832.png" alt="image-20230412211209832" style="zoom:50%;" />

```mysql
#匹配多个实例
select prod_name from products where prod_name regexp '\\([0-9] sticks?\\)' order by prod_name;
#匹配连在一起的4位数字：
select prod_name from products where prod_name regexp '[[:digit:]]{4}' order by prod_name;
select prod_name from products where prod_name regexp '[0-9][0-9][0-9][0-9]' order by prod_name;
#定位符
/*^匹配串的开始。因此，^[0-9\\.]只在.或任意数字为串中第
一个字符时才匹配它们。没有^，则还要多检索出4个别的行（那
些中间有数字的行）。*/
select prod_name from products where prod_name regexp '^[0-9\\.]'

```

# 10创建计算字段

```mysql
select Concat(vend_name, ' (', vend_country, ')') from vendors order by vend_name;
select concat(rtrim(vend_name), ' (', rtrim(vend_country), ')') from vendors order by vend_name;
select concat(rtrim(vend_name), ' (', rtrim(vend_country), ')') as vend_title from vendors by vend_name;
select prod_id, quantity, item_price from orderitems where order_num = 20005;
select prod_id, quantity, item_price, quantity*item_price as expanded_price from orderitems where order_num = 20005;

```

# 11使用数据处理函数

```mysql
select vend_name, Upper(vend_name) as vend_name_upcase from vendors order by vend_name;
select cust_name, cust_contact from customers where soundex(cust_contact) = soundex('Y Lie');
select cust_id, order_num from orders where order_date = '2005-09-01'; #不可靠
select cust_id, order_num from orders where Date(order_date) = '2005-09-01';
select cust_id, order_num from orders where Date(order_date) between '2005-09-01' and '2005-09-30';
select cust_id, order_num from orders where year(oredr_date) = 2005 and month(order_data) = 9;

```

<img src="D:\C++编程学习资料\我的笔记\images\image-20230413221123224.png" alt="image-20230413221123224" style="zoom:50%;" />

<img src="D:\C++编程学习资料\我的笔记\images\image-20230413221133245.png" alt="image-20230413221133245" style="zoom:50%;" />

<img src="D:\C++编程学习资料\我的笔记\images\image-20230413221405619.png" alt="image-20230413221405619" style="zoom:50%;" />

<img src="D:\C++编程学习资料\我的笔记\images\image-20230413221901105.png" alt="image-20230413221901105" style="zoom:50%;" />

# 12 汇总数据

```mysql
select avg(prod_price) as avg_price from products where vend_id = 1003;
select count(*) as num_cust from customers; #* 不省略NULL
select count(cust_email) as num_cust from customers;
select max(prod_price) as max_price from products;
select min(prod_price) as min_price from products;
select sum(quanity) as items_ordered from orderitems where order_num = 20005;
select sum(item_price*quantity) as total_price from orderitems where order_num = 20005;
select avg(distinct prod_price) as avg_price from products where vend_id = 1003;
select count(*) as num_items, min(prod_price) as price_min, max(prod_price) as price_max, avg(prod_price) as price_avg from products;

```

<img src="D:\C++编程学习资料\我的笔记\images\image-20230415100529724.png" alt="image-20230415100529724" style="zoom:50%;" />

# 13 分组数据

```mysql
select count(*) as num_prods from products where vend_id = 1003;
select vend_id, count(*) as num_prods from products group by vend_id; 
select cust_id, count(*) as orders from orders group by cust_id having count(*) >= 2;
select vend_id, count(*) as nums_prods from products where prod_price >= 10 group by vend_id having count(*) >= 2;
select order_num, sum(quantity*item_price) as ordertotal from orderitems group by order_num having sum(quantity*item_price) >= 50
order by ordertotal;

```

# 14 使用子查询

```mysql
select cust_id from orders where order_num IN(select order_num from orderitems where prod_id = 'TNT2');
select cust_name, cust_contact from customers where cust_id IN(10001, 10004);

select cust_name, cust_contact from customers
where cust_id in
(select cust_id from orders where order_num in
(select order_num from orderitems where prod_id = 'TNT2'));

select cust_name, cust_state, (select count(*) from orders where order.cust_id = customers.cust_id)
as orders from customers order by cust_name;

```

 # 15 联结表

```mysql
select vend_name, prod_name, prod_price from vendors, products
where vendors.vend_id = products.vend_id
order by vend_name, prod_name;

select vend_name, pord_name, prod_price
from vendors inner join products on vendors.vend_id = products.vend_id;

# 联结多个表t
select prod_name, vend_name, prod_price, quantity
from orderitems, products, vendors
where products.vend_id = vendors.vend_id
and orderitems.prod_id = products.prod_id
and order_num = 20005;
```

# 16 创建高级联表

```mysql
select cust_name, cust_contact from customers as c, orders as o, orderitems as oi
where c.cust_id = o.cust_id and oi.order = o.cust_id  and prod_id = 'TNT2';

#自联结
select prod_id, prod_name from products where vend_id = (select vend_id from products where prod_id = 'DTNTR');

select p1.priod_id, p1.prod_name from products as p1, products as p2 where p1.vend_id = p2.vend_id and  p2.prod_id = 'DTNTR';

#自然联结
select c.*, o.order_num, o.order_date, oi.prod_id, oi.quantity, OI.item_price
from customers as c, orders as o, orderitems as oi
where c.cust_id = o.cust_id and oi.order_num = o.order_num and prod_id = 'FB';

#外部联结
select customers.cust_id, orders.order_num from customers left outer join
orders on customers.cust_id = orders.cust_id;

select custmoers.cust_name, customers.cust_id, count(order.order_num) as num_ord
from customers inner join orders on customers.cust_Id = orders.cust_id group by customers.cust_id;


```

# 17 组合查询

```mysql
select vend_id, prod_id, prod_price from products where prod_price <= 5
union 
select vend_id, prod_id, prod_price from products where vend_id IN (1001, 1002);

select vend_id, prod_id, prod_price from products where prod_price <= 5 
union all ##MySql不取消重复的行。
select vend_id, prod_id, prod_price from products where vend_id in (1001, 1002);

#在用union组合查询时，只能使用一条order by 子句，必须出现在最后一条select语句之后。
```

# 18 全文本搜索

```mysql
select note_text from productnotes where match(note_text) against('rabbit');
select note_text from productnotes where note_text like '%rabbit%';

#使用查询扩展
select note_text
from productnotes where match(note_text) against('anvils' with query expansion);

#布尔文本搜索
select note_text from productnotes where match(note_text) against('heavy', in boolen mode);

select note_text from productnotes where match(note_text) against('heavy -rope*' in boolean mode);

select note_text from productnotes where match(note_text) against('+rabbit + bait' in boolen mode);

select note_text from productnotes where match(note_text) against('rabbit bait' in boolen mode);

select note_text from productnotes where match(note_text) against('"rabbit bait"' in boolen mode);

select note_text from productnotes where match(note_text) against('>rabbit <carrot' in boolen mode);

select note_text from productnotes where match(note_text) against('+safe +(<combination)' in boolen mode);


```

# 19 数据插入

```mysql
# 插入完整行
insert into customers values(null,
                            'a',
                            'b',
                            'c',
                            null,
                            null); #这种写法不安全

 insert into customers(cust_name,
                      cust_address,
                      cust_state,
                      cust_zip,
                      cust_country
                      ....)
                      values('a',
                            ...);
 
insert into customers(cust_id, cust_contact, cust_email, cust_name, ...)
select cust_id, cust_contact,... from custnew;


```

# 20 更新和删除数据

```mysql
#update语句由3部分组成：
#要更新的表
#列名和它们的新值：
#确定要更新行的过滤条件。
update customers set cust_email = 'elmer@fudd.com' where cust_id = 10005;
update customers set cust_name = 'the fudds',
	cust_email = 'elmer@fudd.com' where cust_id = 10005;

update customers set cust_email = null where cust_id = 10005;
delete from customers where cust_id = 10006;
# 如果省略where子句，它将删除表中每个客户
# 除非确实打算更新和删除每一行，否则绝对不要使用不带WHERE子句的UPDATE或DELETE语句。
#在对update或delete语句where子句前，应该先用selete进行测试，保证它过滤的是正确的记录，以防编写where子句不正确。

```

# 21 创建和操纵表

