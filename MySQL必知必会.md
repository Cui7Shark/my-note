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

