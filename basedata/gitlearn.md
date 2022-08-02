# Git基础

>## 什么是Git
>* Git是文件版本管理工具
>* 类似有：VSS（微软）, SVN （基于CVS）, CVS（老牌）

>## Git特点
>* 分布式
>### 为什么使用Git
>* 问题：多人协同，版本多，容易丢失，回溯

>## Git配置
>* 用户名 + 邮箱
>*     git config --global user.name "name"
>*     git config --global user.email "xxx@yy.com"

>## 本地仓库操作
> ### 本地工作区
>* **选择工作区**
>     * 新建文件夹
>     * 准备文件 （注意文件格式）
>* **新建本地仓库**
>    * 进入文件夹
>    * 打开 Git bash here
>    * 执行：
>    *     git init 
>    * 文件夹下出现  .git 文件夹 
>* **将文件纳入管理（进入暂存区）**
>    * git add 文件名
>    * git add 目录名 
>        * （可以用符号）
>        *  **.** 代表当前目录下所有
>        * \*  通配符，任意字符串
>    * 查看修改：
>    *     git status 
>* **文件提交本地仓库，生成版本**
>    *     git commit -m "版本说明"
>* **提交不同版本到本地仓库**
>    * 修改文件
>    * 查看修改
>    *     git status
>    * 提交版本说明
>    *     git commit -m "新版本说明"
>
>### 版本回溯
>* **快速回溯**（回到上几个版本）
>    *     git reset --hard HEAD^ (6键上方符号，HEAD 大小写都可以)
>    *     git reset --hard HEAD^^ (6键上方符号)（再回溯上上个版本）
>    *     git reset --hard HEAD^*n (6键上方符号)（回溯n个版本）
>* **任意版本回溯**
>      * 查看每个版本的 hash 码，hash码唯一，每次提交生成
>  *     git reflog 
>  *     git reset --hard "hash码"
>* **文件恢复**
>    * 场景一：文件修改错误，没有进入暂存区，没有执行git add
>        *     git checkout -- 文件名
>        * （从本地仓库拉取最新文件覆盖错误文件）
>    * 场景二：文件修改错误，进入了暂存区，没有执行git commit
>        *     git reset HEAD 文件名 （撤销暂存区）
>    * 场景三：文件修改错误，进入了暂存区，执行了git commit，形成了版本
>        * 回溯上个版本
>    * 场景四：文件已经进入远程仓库（无解）
>    * 场景五：文件已经删除
>        *     git checkout -- 文件名（从本地仓库拉取最新文件）
>    * 场景六:想彻底删除文件
>        *     git rm 文件名
>        * 恢复：
>            * 任意版本回溯

>### 分支管理
>#### 创建分支
>* master分支 commit后 自动生成，分支名要有意义，不要用数字
>*     git branch 分支名 （基于命令分支）
>
>#### 切换分支
>*     git checkout 分支名
>* 参数 -b 创建分支并切换到分支
>
>#### 分支合并
>* 修改 dev (被修改的)分支
>* 切换至 master（要合并的） 分支
>*     git merge dev
>
>#### 分支查看
>* 查看所有分支
>*     git branch -a
>    * 带 * 号表示当前分支
>
>#### 分支删除
>*     git branch -d 分支名
> 
>#### 分支冲突
>* 目前状态：dev( v2),master(v2) ,test(v1)
>* 修改 test，从 v1 到 v2
>* 合并 test 到 master
>* 产生冲突，进入 master|MERGING
>    * 编辑文件，然后上传
>    * 或者由市场决定保留哪个分支
>    *     git add .
>    *     git commit -m “版本说明”
>    * 回到master

>## 远程仓库操作
>* 局域网（gitlab）
>* 互联网（github(国外服务器) ，gittee(国内服务器)）
>    * gitee注册
>    * 本地机器和远程仓库进行安全验证
>        * SSH验证：
>            * 本地电脑通过命令生成成对的公钥，私钥
>            * 公钥放到gitee，私钥保存在本地
>
>### 本地机器绑定仓库
>* (本地仓库只能绑定一个远程)
>* 拿到远程仓库地址（leader提供）
>        * 格式：git@gitee.com:仓库.git
>        *     git remote add origin + 地址
>        * 查看：
>        *     git remote -v
>        * 删除：
>        *     git remote rm origin
>
>### 从远程仓库拉取文件
>*     git pull origin master --allow-unrelated-histories
>  * 参数含义：
>    * 允许不相关历史提交，并强制合并
>* 中途需要输入日志
>* 对其他分支没有影响
>
>### 从本地上传仓库
>*     git add
>*     git commit
>*     git push origin master
>
>### 克隆仓库
>*     git clone + 远程仓库地址
				
