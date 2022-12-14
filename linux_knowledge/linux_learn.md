# Linux 基础

># 文件管理
>## 基础知识
>* linux目录结构：
>      * 一切目录从根目录 /开始
>      * /dev 设备文件目标
>      * /home 普通用户家目录
>      * /root root用户家目录
>      * /opt 参数设置目录
>      * /tmp 临时文件目录

>## 文件增加
>* 创建文件:
>      * touch 创建空文件
>* vi
>  * echo +文本 +  >路径 命令将文本输出到指定文件中。文件不存在会创建该文件
>  * 创建目录:
>  * mkdir 创建目录，既可以相对也可以绝对路径
>    * 参数：
>      * -p 当要创建的目录的父目录不存在时，同时创建父目录

>## 文件删除
>* rmdir 删除空目录
>* rm 删除文件（也可以删除目录）
>    * 参数：
>    * -r 递进删除
>    * -f 强制删除（不会询问，慎用）
>    * -rf 强制递进删除（慎用）

>## 文件修改
>* vi + 文件名
>  * 文件不存在时,会创建新文件
>  * 一般模式
>    * 按esc进入命令模式
>    * 按 I,o,a 进入编辑模式
>  * 编辑模式
>    * I 当前光标位置插入
>    * o 在光标下一行输入
>    * a 在光标后输入
>  * 命令模式
>    * : w 保存
>    * : wq 保存退出
>    * : q!  强制退出，不保存，！强制
>    * : set nu 显示行号
>    * : set:nu ! 取消行号显示
>  * 查找字符串
>    * 命令模式：/ + 字符串
>      * 自当前光标位置向上搜索，例如“wenhui”，请使用以下命令：
>        *     /wenhui Enter
>      *向下搜索
>        *     ?wenhui Enter
>* cp [ -option ] 源文件  目标地文件 复制源文件到目标地址,复制时可以改名
>      * -r 可以复制目录下的文件与子目录
>      * -u 源文件较新或没有目标文件时才复制
>      * 复制当前目录下所有文件可以使用 /*
>* mv 源文件 目标   移动文件，也可以用来给目录或者文件改名
>* tar 压缩，解压缩文件
>      * tar -cvf 打包
>      * tar -xvf 解包
>      * tar -zcvf 压缩
>      * tar -zxvf 解压缩
>      * tar -tvf 查看压缩包里内容
>* unzip 解压缩zip文件

>## 文件查看
>* cat +[options] + 文件名  ：
>  * 将一个文件的内容连续输出到屏幕上 
>  * -n 显示行号，包括空行
>  * -b 显示行号，不包括空行
>  * 与其他命令联用：
>    * 统计文本行数：cat 文件名 | wc -l
>    * 查找字符串：cat 文件名 | grep 目标字符串
>    * 查找不存在字符串的行: cat 文件名 | grep -v 字符串
>* tac 
>  * 以cat倒序的方式显示
>* more 
>  * 类似cat，一页一页的显示
>* less 
>  * 类似more，一次显示的内容更少
>* tail
>  * tail [ -n 20 ] 文件 查看文件结尾20行
>  * -n 参数设置显示行数，不写则默认10行
>  * tail -f 文件名 会把文件最末尾的内容打印出来，并持续刷新
>* head
>  * head [ -n 20 ] 文件 查看文件头20行
>  * -n 参数设置显示行数，不写默认10行

>## 查找文件/字符串
>* find 起始地址 -name 文件名
>      * / 从根目录开始
>      * ~ 从属主目录开始
>* grep 字符串 * -r 
>      * 从当前目录向子目录，递进式的在文件中查找字符串
>* pwd 查看当前目录
>* ls 显示当前目录下指定内容
>      * 颜色：
>        *   蓝色：代表目录
>        *   绿色：可执行文件
>        *   红色：压缩文件
>        *   浅蓝色：链接文件
>        *   灰色：普通文件
>  * 参数：
>    *   -a 显示所有文件和目录，包括隐藏文件，以及.和..
>    *   -A 显示所有文件和目录，包括隐藏文件，不包括.和..
>    *   -t 根据时间属性排序
>    *   -l 显示所有文件和目录的完整属性信息
>      *   cd 改变路径
>* 相对路径
>* 绝对路径

># 用户管理
>## 用户增加
>  * 增加用户:
>    *     user add "name"
>  * 增加组 
>    *     group add "name"
>
>## 用户删除
>    * 删除用户:
>      *     userdel
>          * -r 彻底删除，包括家目录和用户邮箱

>## 修改用户
>* 修改用户密码:
>    *     passwd 
>* 修改用户组：
>  *     usermod
>      * 修改用户的默认组 usermod -q 组名 用户名
>      * 把用户加入组 usermod -aG 组名 用户名
>  * su + 账户名 ： 切换账号

>## 查看用户
>    * hostname  + 名称 ：修改终端显示的电脑名称 
>    * id 用户查询，显示用户的UID,GID及所属群组

># 软件管理
>## yum (centos)软件安装工具
>### 安装软件
>*     install + [options] + 软件包名
>  * -y 安装中所有询问默认yes

>### 删除软件
>    *     yum remove 软件名
>
>### 修改软件
>    *     yum update 软件包名
>    *     yum update 升级系统

>### 查找软件
>*     yum search 软件名
>    * 使用YUM查找软件包
>*     yum list
>  * 列出所有可安装的软件包
>*     yum list 软件包名
>  * 列出所指定软件包
>*     yum list installed
>  * 列出所有已安装的软件包
>*     yum list updates
>   * 列出所有可更新的软件包
>*     yum info
>   * 列出所有软件包的信息
>*     yum info updates
>   * 列出所有可更新的软件包信息

>## apt (ubuntu)软件安装工具
>### 安装软件
>   * install + [options] + 软件包名
>   * -f 修复安装
>   * reinstall 软件包名
>     * 重新安装包

>### 删除软件
>*     sudo apt-get remove 软件包名
>  * 删除已安装的软件包（保留配置文件），不会删除依赖软件包，保留配置文件
>*     sudo apt-get purge /sudo apt-get --purge remove
>  * 删除已安装的软件包（不保留配置文件)，删除软件包，同时删除相应依赖软件包。
>*     sudo apt-get clean
>  * 删除已经安装过的的软件安装包

> ### 修改软件
>*     sudo apt-get upgrade
>    * 升级所有已安装的软件包

>### 查找软件
>*     apt search 软件包名
>    * 查找包
>*     apt show 包名
>  * 显示软件包的所有信息

>## rpm

># 进程/IO管理
>* top
>  * 查看cpu，内存使用情况，按进程占用降序，q退出
>* free
>  * 查看内存占用
>* ps -a
>  * 查看进程信息
>* kill  进程pid
>* df
>  * 查看文件系统的磁盘使用情况

># 网络管理
>* netstat
>  * -tlnpu 显示当前系统启用了哪些端口
>* ifconfig
>  * 显示或者设置网卡
>* ping + [ -option ] + ip
>  * -c  数字 ，用于指定测试多少次，不指定数字将会一直进行测试
>* ifup 启动网卡
>* ip addr 查看ip

># 命令符号
>* 管道命令 |
>  * 作用：把一个命令的输出作为输入送给其他命令
>  * 用法：命令1 | 命令2
>
> 
> # 远程操作
> ## Xshell
> ### 特点
> * SSH协议
> ### 文件传输
> * sftp + ip 连接
>   * get 下载
>   * put 上传

> # 常用系统设置
>* 网络设置
>* 环境设置
>* 磁盘管理
>* 系统资源
>* 时间设置
>* 防火墙

