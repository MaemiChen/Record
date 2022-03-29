# 1. 基础教程
## 1. 数据类型
1. 存储在SQLite的数据库值具有以下几种类型

| 存储类 | 描述 |
| ------| ------ |
| NULL | 值是一个NULL值 |
| INTEGER | 带符号整数，根据值的大小存储在1、2、3、4、6、8字节中 |
| REAL | 值是一个浮点数，存储为8字节的IEEE浮点数字 |
| TEXT | 值是一个文本字符串，使用数据库编码(UTF-8/UTF-16BE/UTF-16LE)存储 |
| BLOB | 值是一个blob数据，完全根据他的输入存储 |
| | |  

2. SQLite亲和(Affinity)类型

| TEXT | 描述 |
| ------ | ------ |
| TEXT | 数值型数据被插入前，先转换为文本格式，再插入到目标字段 |
| NUMERIC | 当文本数据被插入到亲缘性为NUMERIC的字段中时，如果转换操作不会导致数据信息丢失以及完全可逆，那么SQLite就会将该文本数据转换为INTEGER或REAL类型的数据，如果转换失败，SQLite仍会以TEXT方式存储该数据。对于NULL或BLOB类型的新数据，SQLite将不做任何转换，直接以NULL或BLOB的方式存储该数据。需要额外说明的是，对于浮点格式的常量文本，如"30000.0"，如果该值可以转换为INTEGER同时又不会丢失数值信息，那么SQLite就会将其转换为INTEGER的存储方式。|
| INTEGRE | 对于亲缘类型为INTEGER的字段，其规则等同于NUMERIC，唯一差别是在执行CAST表达式时。 |
| REAL | 其规则基本等同于NUMERIC，唯一的差别是不会将"30000.0"这样的文本数据转换为INTEGER存储方式。|
| NONE | 不做任何的转换，直接以该数据所属的数据类型进行存储。|
| | |

3. 类型亲和的介绍  
SQlite不强制数据类型的约束，任何数据都可以插入任何列。 可以向一个整型列中插入任意长度的字符串，向bool类型插入浮点数，向字符型插入日期型值。(例外：int primary key 列只能存储64位整数)。    
当向整型列插入字符串时，SQLite会试图将该字符串转换为一个整数，若可以转换，则它将插入整数，否则将插入字符串。

## 2. 逻辑运算符

| 运算符 | 描述 |
| ------ | ------ |
| AND | select * from table_name where id > 5 AND name = 'maemi'; |
| BETWEEN | selcect * from table_name where id between 20 and 50;|
| EXISTS | 使用SQL子查询 select name from table_name where exists ( select name from table_name where id > 20 );|
| IN | select * from table_name where id in (20, 50); |
| NOT IN | |
| LIKE | select * from table_name where name like 'ma%'; 大小写不敏感 |
| GLOB | select * from table_name where name like 'ma*'; 大小写敏感 |
| NOT | |
| OR | |
| IS NULL | |
| IS | |
| IS NOT | |
| \|\| | 连接两个不同的字符串，得到一个新的字符串。 |
| UNIQUE | UNIQUE 运算符搜索指定表中的每一行，确保唯一性（无重复）。 |
| | |

## 3. like和glob

1. like的通配符 "%" (表示0个1个或多个数字字符) "_"(代表单一的数字或字符)

2. glob的通配符 "*"(表示0个1个或多个数字字符) "?"(代表单一的数字或字符)

3. 区别： glob是大小写敏感的

## 4. limit offset
````sql
select * from table_name limit 6;   //限制查询的行数为6行
select * from table_name limit 6 offset 2;  //从第3行开始，查询行数限制为6行 
````

## 5. order by
````sql
select * from table_name order by treatNum;
select * from table_name where id < 5 order by treatNum;    //按照treatNum进行升序排序
//增加DESC为降序排序
````

## 6. group by
````sql
select name , SUM(treatNum) from table_name group by name order by name;
````
## 7. having 
having允许指定条件过滤出现在最终结果中的分组结果
1. having 必须放在group by之后，order by之前

````sql
select name , sum(treatNum) from table_name  group by name having count(name) < 2 order by name;
````
## 8. distinct
distinct消除所有重复的记录，只获取唯一一次记录
````sql
select distinct name from table_name;   
````
# 2. 高级教程

## 1. pragma

## 2. 约束

## 3. join

## 4. union

## 5. 别名

## 6. 触发器


## 常用函数

# 参考链接
1. https://www.cnblogs.com/bamboos/archive/2009/03/03/1402214.html