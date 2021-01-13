# mysql语句

``` mysql
   select CONCAT('truncate TABLE ',table_schema,'.',TABLE_NAME, ';') from INFORMATION_SCHEMA.TABLES where  table_schema in ('dbname');
   #删除数据库
   DROP DATABASE
   #导出数据库
    mysqldump -uroot -p cet_sjzx > /sqlfile/cet_sjzx.sql
```

导出：
1.在命令行里，进入mysql安装根目录下的bin目录下
比如：D:\Program Files\MySQL\MySQL Server 5.0\bin
输入 mysqldump -uroot -p 数据库名 > D:\导出文件名.sql
注意：这里最后不需要加 分号，
按回车后，会让你输入密码，输完即开始导出整个数据库和数据。



带条件导出数据：mysqldump -uroot -p --where='id<5' 数据库名 > D:\导出文件名.sql





2. 如果只导出一张表，只要在数据库名后 按空格并输入表名即可，如:
mysqldump -uroot -p 数据库名 表名> D:\导出文件名.sql


1.在命令行里，输入 mysql -u root -p   按回车
输入正确密码后，进入mysql控制台，将导入的.sql文件放在D盘
根目录下，在mysql控制台命令行里，
输入：show databases; 按回车
查看数据库，确定使用哪个数据库后，

输入：use 数据库名;

再输入：source d:\文件名.sql   按回车
注意：source命令后面不要加 分号
