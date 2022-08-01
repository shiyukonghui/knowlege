# MySQL 基础知识

>## 数据增加
> ### 建表：
>* create table student(
>* sid int primary key,
>* sname varchar(20)not null,
>* grade int);
>* 外键:
>    * foreign key(sid) references 表名(sid)
>
>#### 查看表结构
>*      desc 表名；
>
>### 建库：
>*      create database 库名 default character set = 'utf8'；
>* default character set = 'utf8'必须要加上，否则可能不支持中文；
>* use 数据库名  切换数据库
>
>#### 查看数据库的表结构
>*      show tables；
>
>### 一次插入一条数据：
>* 格式：insert into 表名 ( 字段1, 字段2,……字段n) 
>* values(值1,值2,……值n);
>* 注意：最后一个字段和最后一个值后面没有逗号
>* 全部字段，全部值一 一对应 ：
>*     insert into student(grade,sname,sid) values(3,'lily',1);
>* 字段可以省略不写，如果字段不写，则值的数量必须为全部字段的数量，且顺序必须和表中字段的顺序一致:
>*      insert into student values(2,'lucy',5);
>* 允许为空的字段，可以不写，但字段和值都要不写
>*      insert into student(sid,sname) values(3,'Jack');
>* 字段顺序可以调整，但值的顺序也要相应调整
>*     insert into student(sid,sname,grade) values(4,'Jim',null);
>
>### 一次插入多条数据:
>* insert into 表名 (字段1,字段2,……字段n)
>* values(值1,值2,……值n),
>  * (值1,值2,……值n),
>  * ……
>  * (值1,值2,……值n);
>
>### 增加表的字段
>* 语法：alter table 表名 add 字段名 字段的数据类型;
>    *     alter table stu1 add grade varchar(10);
>### 数据类型
>    * 整型 int
>    * 浮点型 float double decimal
>        *  decimal(16,4)
>            *  16位数字，小数部分4位
>    * 字符型 char varchar
>    * 枚举型 enum 
>        *  enum（“男”，“女”）
>    * 日期型 date datetime time 
>        * date (YYYYMMDD)
>            *  20220925
>        * datetime （YYYY-MM-DD HH:mm:ss）
>            *  2022-09-25 20:35:56
>### 数据关系
>* 主键约束（primary key） 
>        *  主键指的是主关键字，它是表里的一个或多个字段，它的值可以唯一的标识表里的每一条记录。如果你对某个字段设置了主键约束，那么这个字段既不能为空，也不能重复。
>* 非空约束（not null）
>    * 如果你对某个字段设置了这个约束 ，那么这个字段不能为空（null)。
>* 唯一约束（unique） 
>    * 如果你对某个字段设置了这个约束，那么这个字段取值不能重复。
>* 默认值约束（default 默认值）
>    * 如果你给某个字段设置了该约束，那么当你没有给该字段赋值时，它使用默认值；如果你给该字段赋值了，就使用你赋的值。
>* 外键约束（foreign key）
>    * 如果某个字段，它在一张表里是主键，然后它又出现在另外一张表里，那么可以在另外一张表里对它设置外键约束。一旦设立了该约束，那么设置了外键约束字段的取值跟主键之间就有一个参照关系，即外键的取值要参照主键的取值
		

># 数据删除
>## 删除表字段
>* 语法：alter table 表名 drop 字段名;
>*      alter table stu1 drop grade;
>
>## 删除表或者表中数据
>* 语法1：
>   *     drop table 表名;
>   *      drop table stu1;
>     * 注意：**表数据和表结构一起删除**		
>* 语法2：
>*     delete from 表名 [ where 删除条件 ]
>  * 如果不跟条件则会删除表里所有的数据（保留表结构）
>  * 如果跟条件则会删除满足条件的记录
>
>## 删除数据库
>* 语法：
>*     drop database 数据库名

># 数据修改
>## 数据更新
>* 语法：
>   *     update 表名 set 修改的内容 [where 更新条件];
>* 注：
>* 如果不跟更新条件，则表里所有的记录都会更新；
>* 如果跟了更新条件，则只有满足条件的记录会更新；
>* 【示例】将成绩表里所有人的成绩减少2分
>  *     update cjb set cj = cj-2;
>
>## 增加表的字段
>* 语法：
>  *     alter table 表名 add 字段名 字段的数据类型;
>  *     alter table stu1 add grade varchar(10);
>
>## 修改字段的数据类型及约束
>        *      alter table <表名> modify <字段名> <数据类型>

># 数据查询
>## 单表查询
>* 查询格式：
>     * **select** [distinct] *|字段1，字段2……字段n 
>         *  select子句的作用是过滤出满足需求的列（字段）；
>         *  distinct 去重
>             *  多字段时只去除完全相同的
>         *  符号*代表所有字段
>         *  聚合函数：
>             *  常用的聚合函数：
>                 *  count() 统计个数
>                 *  avg() 求平均值
>                     *  round(avg() , number)  number可以设置保留位数
>                 *  sum() 求和
>                 *  max() 求最大值
>                 *  min() 求最小值
>     * **from 表名** 
>         *  from子句的作用是明确数据的来源；
>     * **where 查询条件**
>         *   where子句的作用是过滤出满足需求的行（记录），只能对分组之前的数据做过滤，聚合函数不能出现在where子句里
>         *  经常用到的关系运算符：=  !=  >  >=  <  <=
>         *  支持多条件
>             *  与 and
>             *  或者 or
>             *  非 not
>             *  between （较小边界） and  （较大边界）在……之间
>                 *  包括边界
>             *  in 在一个集合里任选其一
>             *  not between and 不在某个范围里
>                 *  不包括边界
>             *  not in 不在某个集合里
>         *  支持模糊查询
>             *  =号换成  like 
>             *  通配符：
>                 *  % 匹配任意多个任意字符（可以是0个）
>                 *  _ 匹配一个字符
>         *  空值查询
>             *  is null  是空值
>             *  is not null 不是空值
>     * **group by 分组字段** ：
>         *  按某个字段做分组
>         *  注意：
>             *  一旦按照某个字段分组，那么select后只出现分组的字段和聚合函数；(1055错误)
>             *  如果有where条件，那么一定在group by的前面，它会在分组之前先过滤不满足条件的数据，然后在剩下的满足条件的数据中，进行分组；
>             *  如果有order by排序，那么一定在group by的后面，是对分组后的结果集中的数据的排序；
>             *  如果分组后，想要筛选符合条件的组，那么需要在group by后使用having，而不是where;
>     * **having 过滤条件** 
>         *  对分组之后的数据做进一步过滤，是针对组的过滤，能使用聚合函数来过滤
>         *  having和where的区别：
>             *  书写位置不同，where在groupby之前，having在groupby之后
>             *  执行顺序不同，where和group同时出现是先筛选后分组，group by和having是先分组后筛选，也就是where执行在分组之前，having执行在分组之后。
>             *  where后面的条件只能是表中的字段，having可以是聚合函数或者运算函数
>     * **order by 字段 [asc|desc]：** 
>         *  asc 升序
>         *  desc 降序
>         *  默认升序
>     * **限制结果输出（limit）：**
>         *  使用limit m,n  m <n
>         *   使用limit n 则默认从0开始
				
>## 复杂查询：
>### 子查询
>* 查询嵌套，指的是查询语句里又嵌套了若干个小的查询。那么子查询对应的主查询，也叫做父查询
>     * 注意：
>         *  查询要用小括号括起来；
>         *  子查询经常用在where后做条件的一部分
>         *  如果子查询返回多行，要用in进行连接，而不是=
>         *  父查询 where 字段1 中的字段1 为子查询 select 后的 字段1，即将子查询的结果作为集合给父查询作为条件
>             * 例子：
>             * select * from xsb where xh in( select xh from cjb where cj>80 );
>
>### 关联查询：
>* 通过笛卡尔积运算（将第一张表里的每一行跟第二张表里的每一行一一组合，从而生成一张大的表的过程）连接数据表
>* 步骤：
>    *  1、**连接表：**
>        * 例子：
>        *     select * from xsb , cjb;
>    *  2、过滤垃圾数据：
>        *  通过添加关联条件的方式进行垃圾数据的过滤
>        *  **内连接:**
>            * 只返回满足关联条件的结果集
>            *      select * from xsb , cjb where xsb.xh=cjb.xh;
>            * 或者：
>            *     select * from xsb inner join cjb on xsb.xh=cjb.xh;
>        *  **外连接**
                > *  左连接
                >*  left join 返回满足关联条件的结果集，把left join左边的那张表完整的展示出来，右边的那张表里不满足关联条件的字段位置补空值（null）
                > *  右连接
                >*  right join 返回满足关联条件的结果集，把right join右边的那张表完整的展示出来，左边的那张表里不满足关联条件的字段位置补空值（null）
                > *  全连接
                >*  full join 返回满足关联条件的结果集，把full join两边的表完整的展示出来，两边不满足关联条件的字段位置补空值（null）
>        *  可以在连接时给表赋予别名 格式 ：表名 + 空格 + 别名 即可
>        *  注意：
                > *  对于同名字段，我们需要在字段名前面加上表名做前缀以示区分（建议所有字段都加上表名）；
                > *  可以通过and连接其他的条件（表的连接简写方式里），对于inner join的这种标准写法，我们把条件放在where子句里。
>    *  where,group by, having,order by ,limit用法与单表相同
>    *  多表查询可以与子查询结合
>* if 查询
>    *  格式: 
>        *  查询 if(condition,a,b) 如果condition为真，则返回a值，否则返回b值。
>    *  condition ：
>        *  大于 >
>        *  小于 <
>        *  不等于 ！=
>        *  大于等与>=
>        *  小于等与<=
>    *  例子：
>        *  select if (70<60,"不及格" , if(70<80,"良好","优秀") ) from cjb;
>    *  if 可以嵌套进行多级判断
>* case when查询
>    *  格式：
>        *   case列名 
>        *  when 条件1 then 返回值1 
>        *  when 条件2 then 返回值2 ...... 
>        *  when 条件n then 返回值n 
>        *  else返回默认值 end
>    *  例子：
>        *  select id ,class ,(case 
>        *  class when "一班" then "jwclass1" 
>        *  when "二班" then "jwclass2" 
>        *  when "三班" then "jwclass3" 
>        *  else "其他" end
>        *  ) jwclass_result from jw04_stu8; jwclass_resul
>* 查看当前数据库
>    *      select database();
						
					
					
				
				
						
			
					
					
					
				
				
			
