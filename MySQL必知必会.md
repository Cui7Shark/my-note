# 第1-5章

# 基础的一些指令

```mysql
#启动mysql
net start mysql80
#停止mysql
net stop mysql80
#登录mysql
mysql -uroot -pabc123
#选择数据库
use mydatabase;
#显示数据库
show mydatabase;
#显示数据库中的可用表
showtables;
#检索单个列
select prod_name from products;
#检索多个列
select prod_id, prod_name, prod_price from products;
#检索所有列
select*from products
#检索不同行
select vend_id from products;
#DISTINCT 关键字， 指示MySQL只返回不同的值
select DINSTINCT vend_id from products;
#limit 5指示MYSQL返回不多于5行
select prod_name from products limit5;
#LIMIT 5, 5指示MySQL返回从行5开始的5行。第一个数为开始位置，第二个数为要检索的行数
select prod_name from products limit5,5;
#使用完全限定的列名
select products.prod_namefrom products;
#使用完全限定的表名
select products.prod_namefrom crashcourse.products;
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

- - 为什么使用NOT？对于简单的WHERE子句，使用NOT确实没有什么优势。但在更复杂的子句中，NOT是非常有用的。例如，在与IN操作符联合使用时，NOT使找出与条件列表不匹配的行非常简单

# 8  用通配符进行过滤

- ***\**\*谓词\*\**\***    操作符在它作为谓词时，不是操作符。

## ***\**\*1.百分号（%）通配符\*\**\***

- % 表任何字符出现任意次数。

```mysql
SELECT prod_id, prod_name
From products
WHERE prod_name LIKE'jet%';
```

- - 区分大小写

```mysql
• SELECT prod_id, prod_name
From products
WHERE prod_name LIKE'%anvil%';

SELECT prod_id, prod_name
From products
WHERE prod_name LIKE's%e';#找s起头e结尾的产品
    ◦ % 不能匹配NULL值作为产品名的行。
```

## 2.下划线（_） 通配符

- 只能匹配单个字符

```mysql
SELECT prod_id, prod_name
From products
WHERE prod_name LIKE'_ton anvil';
```

# 9章 用正则表达式进行搜索

- 正则表达式是用来匹配文本的特殊的串（字符集合）

```mysql
SELECT prod_name
FROM products
WHERE prod_name REGEXP'1000'
ORDERBY prod_name;

SELECT prod_name
FROM products
WHERE prod_name REGEXP'.000'#.是正则表达式语言中一个特殊的字符。它表示匹配任意一个字符，因此，1000和2000都匹配且返回。
ORDERBY prod_name;
/*如果 regexp 需要匹配整个字段, 最好使用^$ 
    重要区别
like 整个字段匹配表达式成功才返回
regexp 部分字符匹配表达式成功即可返回
*/

#进行OR匹配
SELECT prod_name
FROM products
where prod_name regexp'1000|2000'
orderby prod_name;

select prod_name
from prodcuts
where prod_name regexp'[123] Ton'
orderby prod_name;

select prod_name
from prodcuts
where prod_name regexp'[^123] Ton'
orderby prod_name;

#匹配范围
select prod_name from products where prod_name regexp'[1-5] Ton'orderby prod_name;
#匹配特殊字符
select vend_name from vendors where vend_name regexp'\\.'orderby vend_name;
#匹配字符类



#匹配多个实例
select prod_name from products where prod_name regexp'\\([0-9] sticks?\\)'orderby prod_name;
#匹配连在一起的4位数字：
select prod_name from products where prod_name regexp'[[:digit:]]{4}'orderby prod_name;
select prod_name from products where prod_name regexp'[0-9][0-9][0-9][0-9]'orderby prod_name;
#定位符
/*^匹配串的开始。因此，^[0-9\\.]只在.或任意数字为串中第
一个字符时才匹配它们。没有^，则还要多检索出4个别的行（那
些中间有数字的行）。*/
select prod_name from products where prod_name regexp'^[0-9\\.]'

```

# 10创建计算字段

```mysql
select Concat(vend_name,' (', vend_country,')')from vendors orderby vend_name;
select concat(rtrim(vend_name),' (', rtrim(vend_country),')')from vendors orderby vend_name;
select concat(rtrim(vend_name),' (', rtrim(vend_country),')')as vend_title from vendors by vend_name;
select prod_id, quantity, item_price from orderitems where order_num =20005;
select prod_id, quantity, item_price, quantity*item_price as expanded_price from orderitems where order_num =20005;
```

# 11使用数据处理函数

```mysql
select vend_name, Upper(vend_name)as vend_name_upcase from vendors orderby vend_name;
select cust_name, cust_contact from customers where soundex(cust_contact)= soundex('Y Lie');
select cust_id, order_num from orders where order_date ='2005-09-01';#不可靠
select cust_id, order_num from orders whereDate(order_date)='2005-09-01';
select cust_id, order_num from orders whereDate(order_date)between'2005-09-01'and'2005-09-30';
select cust_id, order_num from orders whereyear(oredr_date)=2005and month(order_data)=9;
```

# 12 汇总数据

```mysql
selectavg(prod_price)as avg_price from products where vend_id =1003;
selectcount(*)as num_cust from customers;#* 不省略NULL
selectcount(cust_email)as num_cust from customers;
selectmax(prod_price)as max_price from products;
selectmin(prod_price)as min_price from products;
selectsum(quanity)as items_ordered from orderitems where order_num =20005;
selectsum(item_price*quantity)as total_price from orderitems where order_num =20005;
selectavg(distinct prod_price)as avg_price from products where vend_id =1003;
selectcount(*)as num_items,min(prod_price)as price_min,max(prod_price)as price_max,avg(prod_price)as price_avg from products;
```



# 13 分组数据

```mysql
selectcount(*)as num_prods from products where vend_id =1003;
select vend_id,count(*)as num_prods from products groupby vend_id;
select cust_id,count(*)as orders from orders groupby cust_id havingcount(*)>=2;
select vend_id,count(*)as nums_prods from products where prod_price >=10groupby vend_id havingcount(*)>=2;
select order_num,sum(quantity*item_price)as ordertotal from orderitems groupby order_num havingsum(quantity*item_price)>=50
orderby ordertotal;
```

# 14 使用子查询

```mysql
select cust_id from orders where order_num IN(select order_num from orderitems where prod_id ='TNT2');
select cust_name, cust_contact from customers where cust_id IN(10001,10004);

select cust_name, cust_contact from customers
where cust_id in
(select cust_id from orders where order_num in
(select order_num from orderitems where prod_id ='TNT2'));

select cust_name, cust_state,(selectcount(*)from orders whereorder.cust_id= customers.cust_id)
as orders from customers orderby cust_name;

```

# 15 联结表

```mysql
select vend_name, prod_name, prod_price from vendors, products
where vendors.vend_id= products.vend_id
orderby vend_name, prod_name;

select vend_name, pord_name, prod_price
from vendors innerjoin products on vendors.vend_id= products.vend_id;

# 联结多个表t
select prod_name, vend_name, prod_price, quantity
from orderitems, products, vendors
where products.vend_id= vendors.vend_id
and orderitems.prod_id= products.prod_id
and order_num =20005;
```

# 16 创建高级联表

```mysql
select cust_name, cust_contact from customers as c, orders as o, orderitems as oi
where c.cust_id= o.cust_idand oi.order= o.cust_id  and prod_id ='TNT2';

#自联结
select prod_id, prod_name from products where vend_id =(select vend_id from products where prod_id ='DTNTR');

select p1.priod_id, p1.prod_namefrom products as p1, products as p2 where p1.vend_id= p2.vend_idand  p2.prod_id='DTNTR';

#自然联结
select c.*, o.order_num, o.order_date, oi.prod_id, oi.quantity, OI.item_price
from customers as c, orders as o, orderitems as oi
where c.cust_id= o.cust_idand oi.order_num= o.order_numand prod_id ='FB';

#外部联结
select customers.cust_id, orders.order_numfrom customers leftouterjoin
orders on customers.cust_id= orders.cust_id;

select custmoers.cust_name, customers.cust_id,count(order.order_num)as num_ord
from customers innerjoin orders on customers.cust_Id= orders.cust_idgroupby customers.cust_id;
```



# 17 组合查询

```mysql
select vend_id, prod_id, prod_price from products where prod_price <=5
union
select vend_id, prod_id, prod_price from products where vend_id IN(1001,1002);

select vend_id, prod_id, prod_price from products where prod_price <=5
unionall##MySql不取消重复的行。
select vend_id, prod_id, prod_price from products where vend_id in(1001,1002);

#在用union组合查询时，只能使用一条order by 子句，必须出现在最后一条select语句之后。
```

# 18 全文本搜索

```mysql
select note_text from productnotes wherematch(note_text) against('rabbit');
select note_text from productnotes where note_text like'%rabbit%';

#使用查询扩展
select note_text
from productnotes wherematch(note_text) against('anvils'withquery expansion);

#布尔文本搜索
select note_text from productnotes wherematch(note_text) against('heavy',in boolen mode);

select note_text from productnotes wherematch(note_text) against('heavy -rope*'inbooleanmode);

select note_text from productnotes wherematch(note_text) against('+rabbit + bait'in boolen mode);

select note_text from productnotes wherematch(note_text) against('rabbit bait'in boolen mode);

select note_text from productnotes wherematch(note_text) against('"rabbit bait"'in boolen mode);

select note_text from productnotes wherematch(note_text) against('>rabbit <carrot'in boolen mode);

select note_text from productnotes wherematch(note_text) against('+safe +(<combination)'in boolen mode);
```



# 19 数据插入

```mysql
# 插入完整行
insertinto customers values(null,
                            'a',
                            'b',
                            'c',
                            null,
                            null);#这种写法不安全

insertinto customers(cust_name,
                      cust_address,
                      cust_state,
                      cust_zip,
                      cust_country
                      ....)
                      values('a',
                            ...);
 
insertinto customers(cust_id, cust_contact, cust_email, cust_name, ...)
select cust_id, cust_contact,...from custnew;

```

# 20 更新和删除数据

```mysql
#update语句由3部分组成：
#要更新的表
#列名和它们的新值：
#确定要更新行的过滤条件。
update customers set cust_email ='elmer@fudd.com'where cust_id =10005;
update customers set cust_name ='the fudds',
    cust_email ='elmer@fudd.com'where cust_id =10005;

update customers set cust_email =nullwhere cust_id =10005;
deletefrom customers where cust_id =10006;
# 如果省略where子句，它将删除表中每个客户
# 除非确实打算更新和删除每一行，否则绝对不要使用不带WHERE子句的UPDATE或DELETE语句。
#在对update或delete语句where子句前，应该先用selete进行测试，保证它过滤的是正确的记录，以防编写where子句不正确。

```

# 21 创建和操纵表

```mysql
createtable customers
(
    cust_id         int         notnullauto_increment,
    cust_name       char(50)    notnull,
    cust_address    char(50)    null,
    cust_city       char(50)    null,
    ...
)engine=innodb;
#指定默认值
createtable orderitems
(
    order_num   int     notnull,
    order_item  int     notnull,
    prod_id     char(10)    notnull,
    quantity    int     notnulldefault1,# 未给出数量时，使用数量1.
    item_price  decimal(8,2)    notnull,
    primarykey(order_num, order_item)
)engine=innodb;#引擎类型
#innodb：一个可靠的事务处理引擎，不支持全文本搜索
#memory:功能等同于myisam， 但由于数据存储在内存中，速度很快
#myisam：一个性能极高的引擎，支持全文本搜索，但不支持事务处理
altertable vendors add vend_phone char(20);#给表vendors增加一个名为vend_phone的列， 明确数据类型。
altertable vendors dropcolumn vend_phone;#删除列

#删除表 永久
droptable customers2;
#重命名
renametable customers2 to customers;
```



# 22 使用视图

```mysql
#视图是一个虚拟的表，帮助我们不必了解表的创建和表的联结，也可以查询我们想要的数据。
#视图仅仅是用来查看存储在别处的数据的一种设施，视图本身不包含数据，因此他们返回的数据是从其他表中检索出来的。
#在添加或更改这些表中的数据时，视图将返回改变过的数据。
#利用视图简化复杂的联结
createview productcustomers asselect cust_name, cust_contact, prod_id from customers, orders,
orderitems where customers.cust_id= orders.cust_idand orderitems.order_num= orders.order_num;

select cust_name, cust_contact from productcustomers where prod_id ='TNT2';
#视图极大地简化了复杂SQL语句的使用，利用视图，可一次性编写基础SQL，然后根据需要多次使用。
#用视图重新格式化检索出的数据
select concat(RTrim(vend_name),' (', RTrim(vend_country),')')as vend_title from vendors orderby vend_name;

createview vendorlocations asselect concat(RTrim(vend_name),' (', RTrim(vend_country),')')
as vend_title from vendors orderby vend_name;
#过滤
createview customeremaillist asselect cust_id. cust_name, cust_email from customers where cust_email isnotnull;
#
createview orderitemsexpanded asselect order_num, prod_id, quantity, item_price,
            quantity*item_price as expanded_price from orderitems;

select*from orderitemsexpanded where order_num =20005;
#视图应该用于检索而不是更新。
```







# 23使用存储过程

```mysql
#执行存储过程
call productpricing(@pricelow,
                  @pricehigh,
                    @priceaverage);
#创建存储过程                     
createprocedure productpricing()
begin
    selectavg(prod_price)as priceaverage
    from products;
end;
call productpricing();
dropprocedure productpricing;
#使用参数
createprocedure productpricing(
    out p1 decimal(8,1),
    out ph decimal(8,2),
    out pa decimal(8,2),
)
begin
    selectmin(prod_price)
    into p1
    from product;
    selectmax(prod_price)
    into ph
    from product;
    selectavg(prod_price)
    into pa
    from product;
end;
##
call productpricing(@pricelow,
                   @pricehigh,
                   @priceaverage);
# 所有mysql变量都必须以@开始
select@priceaverage;
##
select@pricehigh,@pricelow,@priceaverage;
##
createprocedure ordertotal(
    in onumber int,        #传入存储过程
    out ototal decimal(8,2) #传出存储过程
)
begin
    selectsum(item_price*quantity)
    from orderitems
    where order_num = onumber
    into ototal;
end;
##
call ordertotal(20005,@total);
select@total;
# 检查存储过程
showcreateprocedure ordertotal;
```



# 24 使用游标

- 游标是一个存存储在Mysql服务器上的数据库查询，他不是一条select语句，而是被语句检索出来的结果集。在存储了游标之后，应用程序可以根据需要滚动或浏览其中的数据。

```mysql
createprocedure processorders()
begin
    declare ordernumbers cursorforselect order_num from orders;
end;
#declare (声明)语句用来定义和命名游标
open ordernumbers;
close ordernumbers;
##
createprocedure processorders()
begin
    declare the cursor
    for
    select order_num from orders;
    
    open ordernumbers;
    fetch ordernumbers into o;
    close ordernumbers;
end;
##
createprocedure processorders()
begin
    declare done boolen default0;
    declare o int;
    declare t decimal(8,2);
    declare ordernumbers cursorforselect order_num from orders;
    declarecontinuehandlerforsqlstate'02000'set done =1;
    
    createtableifnotexists ordertotals (order_num int, total decimal(8,2));
    
    open ordernumbers;
    
    repeat
        fetch ordernumbers into o;
        
        call ordertotal(o,1, t);
        
        -- insert order and total into ordertotals
        insertinto ordertotals(order_num, total)values(o, t);
        until done endrepeat;
        close ordernumbers;
end;
##  
```

​    

# 结束
