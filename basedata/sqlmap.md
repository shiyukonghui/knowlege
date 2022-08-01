# Sqlmap的使用

>* **Sqlmap工具**
>   *  适用于sql注入
>   *  原理:
>      *  通过参数,使用拥有的大量模板进行sql注入
>   *  使用方法:
>      *  给出网站
>        *  -u http://localhost/sqli-labs-php7-master/Less-1/?id=3
>      *  **给出注入点**
>        *  ?id=3
>      *  选择参数
>        *  例如:
>          *  --current-db 获取数据库名称
>          *  --passwords 尝试破解数据库密码
>   *  **参数类型**
>      *  **第一类:偷取数据库配置信息**
>        *  --banner 获取数据库版本信息
>        *  --current-db 获取数据库名称
>        *  --current-user 获取数据库管理系统当前用户
>        *  --hostname 获取数据库服务器的主机名称
>        *  --users 枚举数据库管理系统用户
>        *  --privileges 枚举数据库管理系统用户的权限
>        *  --is-dba 检测DBMS当前用户是否DBA
>        *  --passwords 尝试破解数据库密码,枚举数据库管理系统用户密码哈希
>      *  **第二类:日志等级参数和风险参数**
>        *  **七种日志参数:**
>          *  日志等级:0~6
>          *  -v 数字
>          *  0 最简略,只显示报错严重信息,6最详细,默认1 ,2显示debug,3显示注入参数,4增加显示请求信息,5增加显示http响应头,6增加显示响应页面
>          *  参数：--output-dir = 指定路径
>        *  **四种风险参数: 0~3**
>          *  --risk 数字
>          *  0,1较安全的语句(select)作为注入点
>          *  2(用insert 作为注入点)
>          *  3(增加使用updata delect语句作为注入点)
>          *  风险参数越高越容易破解,对网站伤害也越深(对数据造成破坏)
>            *  测试环境 都可以
>            *  生产环境 <=1
>        *  **五种探测等级**
>          *  ‘--level’ （1 - 5，默认为1）
>      *  **第三类:偷取数据(表 字段名字 数据内容)**
>        *  --privileges 是否有权限
>        *  --dbs所有数据库
>        *  -D 指定数据库 --tables 下所有的表
>          *  示例 :
>            *  url --tables -D "数据库名"
>        *  指定表中字段
>          *  示例:
>            *  url --columns -D "数据库名"
>        *  指定表中数据
>          *  示例:
>            *  url --dump -D "" -T "" -C ""
>            *  url --sql-query="select * from 表名"
>      *  **第四类:模拟请求数据**
>        *  破解POST请求:
>          *  --data 指定的数据会被用post方式提交
>            *  示例:
>            *  --data="uname=1&passwd=2" 
>          *  --user-agent"模拟请求端(手机浏览器等等)"
>          *  --level = 3 模拟客户端信息,模拟身份等级
>          *  --method=post  可以指定请求方式，如GET、POST、PUT等
>          *  --cookie :使用指定的cookie
>          *  --mobile 使用手机的user agent
>            * 示例:python sqlmap.py -u "http://localhost/sqli-labs-php7-master/Less-11/
>      *  **第五类:sqlmap和burp联合使用**
>          *  需求原因:网站页面数量巨大
>          *  **使用原理:**
>            *  burp爬网---->保存到log文件
>            *  sqlmap解析log文件发起SQL注入
>          *  **使用方法:**
>            *  burp -->project options
>            *  Logging --->Spider 勾选Requests,将爬网数据保存为 .log文件
>            *  burp爬取网站
>            *  sqlmap 参数:
>              *   python sqlmap.py -l "burp日志路径" --smart(智能跳过) --batch(批处理,默认输入回车或者y) + 之前的攻击参数
>              *  批量执行sql注入
>   *  **常用参数:**
>      *  -h 查看帮助
>      *  -v 日志等级
>      *  --version查看版本
>      *  --dbms 指定数据库类型
>        *  示例: python sqlmap.py -u "http://localhost/sqli-labs-php7-master/Less-1/" --dbms mysql
>      *  --banner 获取数据库版本和类型
>      *  --current-db 获取攻击地址的数据库的名称
>      *  --flush-session (sqlmap对扫描模板有缓存)
>      *  --crawl  从目标URL开始爬取目标站点并收集可能存在漏洞的URL
>        *  示例: python sqlmap.py -u "http://localhost/sqli-labs-php7-master/" --crawl=3
>      *  --threads=数字  : 最大的HTTP（S）请求并发量（默认为1），最大10, sqlmap\lib\core\settings.py,修改MAX_NUMBER_OF_THREADS可更改最大线程数
>        *  示例: python sqlmap.py -u "http://localhost/sqli-labs-php7-master/Less-1/?id=1" --common-tables -D "security" --threads=5
>      *  --os-shell 交互式的操作系统的shell (linux下)
>      *  --os-cmd = OSCMD 执行操作系统命令(windows)
>        *  示例:python sqlmap.py -u "http://localhost/sqli-labs-php7-master/Less-1/?id=1" --os-cmd="ifconfig"
>      *  --proxy="代理ip:端口" 使用代理
>      *  --sql-query=QUERY 执行指定的SQL语句
>        *  示例:python sqlmap.py -u "http://localhost/sqli-labs-php7-master/Less-1/?id=1" --sql-query="select * from users"

>   *  其他
>   *  ‘--’ 和 ‘-’ 的区别
>      *  ‘--’ 后一般跟完整的单词
>      *  ‘-’ 后一般跟单词的缩写，即单个字母