# MySQL 的常见错误

>* 常见错误:
>    * Mysql
>        * 1062 --Duplicate entry ‘...‘ for key ‘PRIMARY
>          * 主键重复
>        * 1055 
>          * 参考解析：
>            * https://www.cnblogs.com/haoyul/p/9882853.html
>          * -- // 关闭 session级
>            *     SET SESSION sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));
>          * -- // 关闭 GLOBAL级
>            *     SET GLOBAL sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));