# Docker学习

>## docker 安装
>* 使用官方的安装脚本安装
>  *     curl -fsSL https://get.docker.com/ | sh
>    * -f 连接失败时不显示http错误
>    * -s 静音模式，不输出任何东西
>    * -S 显示错误
>    * -L 跟随重定向
>* 安装docker-composer的方法:
>  * 方法1:
>    *     apt-get install docker-compose-plugin
>  * 方法2:
>    *     apt-get -yqq install aptitude
>    *     aptitude -y install python3-pip
>    *     pip3 install docker-composer
>      * 使用时输入：
>      *     python3 docker-composer

>## docker启动服务
>* centos:
>  *     systemctl start docker
>* ubuntu: 
>  *     service docker start   
>* 守护进程重启
>  * ubuntu: 
>    *     sudo systemctl daemon-reload
>* 重启docker服务:
>  * centos:
>    *     systemctl restart  docker
>  * ubuntu: 
>    *     service docker restart
>* 关闭docker服务:
>  * ubuntu: 
>    *     service docker stop
>  * centos: 
>    *     systemctl stop docker
>* 问题：
>  * docker 权限问题：
>    * ot permission denied while trying to connect to the Docker daemon socket at …
>    * docker需要root权限
>* 查看帮助：
>  *     docker command --help


>## docker的组成:(仓库，镜像，容器)
>### 仓库（Repository）
>  * 仓库特点：
>   * 仓库是一个提供集中的存储、分发镜像的服务站。
>   * Docker仓库分为公有和私有。
>   * 公有的Docker仓库是 Docker Hub
>
>#### 部署企业内部私有仓库（registry）
>  * 下载registry的镜像
>  * 在宿主机中新建一个用于存放本地文件的目录
>  * 运行registry镜像
>    *     docker run -d -p 5000:5000 -v /home/images:/var/lib/registry --restart=always registry
>    *     --restart=always：在容器退出时总是重启容器，所以在docker ps查看容器时，其可能的状态只有Up或Restarting两种状态。
>    * 5000端口：registry默认是开放5000端口供宿主机映射。
>    * 默认情况下，仓库会被创建在容器的 /var/lib/registry 目录下。可以通过 -v 参数来将镜像文件映射存放在本地的指定路径/src/images下。
>
>#### 上传镜像
>* 标记
>    * 示例：
>    *     docker tag hello-world localhost:5000/myhello
>* 上传：
>  * 示例：
>  *     docker push localhost:5000/myhello
>  * 会在/src/images下，生成docker/registry/v2/repositories的目录，然后存放myhello镜像。
>* 下载：
>  * 示例：
>  *     docker pull localhost:5000/myhello

>### 镜像（image）
>#### 镜像特点：
>  * Docker 镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，
>  * 还包含了一些为运行时准备的一些配置参数（如匿名卷、环境变量、用户等）。
>  * 镜像不包含任何动态数据，其内容在构建之后也不会被改变。
>  * Docker 镜像是用于创建 Docker 容器的模板。
>
>#### 镜像操作：
>##### 查找镜像
>*     docker search php
>
>##### 查看本地镜像
>*     docker images
>  * REPOSITORY：表示镜像的仓库源
>  * TAG：镜像的标签
>  * IMAGE ID：镜像ID
>  * CREATED：镜像创建时间
>  * SIZE：镜像大小
>
>##### 拉取镜像
>*     docker pull php
>  * 默认拉取最新版本
>* 拉取指定版本：
>  *     docker pull 镜像名:版本号
>
>##### 离线(本地)加载
>*     docker load < /home/centos-lamp.tar（地址）
>
>##### 导出镜像
>*     docker save -o 文件名.tar  镜像名：版本
>    * 将指定版本镜像保存成 tar 归档文件
>
>##### 标记镜像
>*     docker tag 0b8d572d1c7d（镜像hash） nickistre/centos-lamp（标记名）
>
>##### 删除本地镜像
>      *     docker rmi 镜像id
>
>#### 制作镜像:
>##### 从镜像生成镜像（使用Dockerfile文件）
>* 原理：
>  * Docker的镜像是一层一层组成的。
>  * Dockerfile 是一个文本文件，包含了指令(Instruction)，每一条指令构建一层。指令的内容为描述该层应当如何构建。
>* 操作步骤：
>  * 在目录下新建Dockerfile文件（文件名必须为Dockerfile）
>  * 打开Dockerfile文件，输入指令
>  * 示例：
>  *     FROM docker.io/webdevops/php-apache-dev COPY hello.php /app 
>  *     RUN echo "this is another file.named monday" > /app/monday.html 
>  *     RUN mkdir /app/testrun 
>  *     RUN echo "this is another file.named tuesday.html" > /app/testrun/tuesday.html
>  * 指令含义：
>    * –  FROM 就是指定基础镜像，一个 Dockerfile 中 FROM 是必备的指令，并且必须是第一条指令。
>    * – RUN用来执行命令行命令的。RUN一次就会形成一个记录也即一个新的层，因此可以用&&符号连接多条指令，来形成一个层
>    * – COPY是复制文件的指令。
>    * – echo命令将文本输出到monday.html文件中。
>  * 执行Dockerfile文件
>    *     docker build –t 镜像名：版本号 .   （注意最后有个点表示上下文的路径）
>      * -t ：指定要创建的目标镜像名
>  * **查看镜像的层结构**：
>    *     docker history 镜像名：版本号（如果有不同版本就带上版本号）
>
>##### 从容器生成镜像:
>* 示例：
>  * 下载官方httpd的镜像。或者直接本地装载已经下好的images
>  * 运行生成容器，查找并记录页面发布目录以及apache的端口等信息
>    * 例如：在/usr/local/apache2/conf/httpd.conf文件中，找到端口以及页面发布目录信息。
>  * 将容器里的index.html拷贝出来后删除容器里的文件index.html。
>  * 修改文件后（或者将制作好的网站文件），拷贝进容器
>  * 打开浏览器查看页面是否能正常打开
>  * 将这个容器保存成镜像
>    *     docker commit -a "作者信息" -m "备注信息"  容器hash  新的镜像名
>* 查看生成镜像过程的操作
>  * docker diff 生成的镜像名
>    * 步骤前的标注含义：
>      * – C：change 
>      * – D：Delete 
>      * – A：Add
>  * 缺点：
>    * 生成镜像时实际修改的内容比操作的内容更多，使用commit，会把这些变更都打包进镜像中，会使镜像变得越来越冗余和臃肿。
>    * 使用 docker commit 意味着所有对镜像的操作都是黑箱操作，生成的镜像也被称为黑箱镜像，是除了制作镜像的人知道执行过什么命令、怎么生成的镜像，别人根本无从得知。
>    * 而且，即使是这个制作镜像的人，过一段时间后也无法记清具体的操作步骤。虽然 docker diff 或许可以告诉得到一些线索，但是远远达不到可以确保生成一致镜像的地步。这种黑箱镜像的维护工作是非常繁琐痛苦的。
>    * 所以一般使用Dockerfile的做法。

>### 容器（container）
>#### 容器特点
>* 镜像（Image）和 容器（Container）的关系，就像是面向对象程序设计中的 类 和 实例 一样，镜像是静态的定义，容器是镜像运行时的实体。
>* 容器内的进程是运行在一个隔离的环境里的，就像独立于宿主机的另一台机器。
>* 所有容器的文件写入操作，都应该使用 数据卷（Volume）、或者绑定宿主目录。
>
>#### 容器操作
>##### 容器运行
>* 守护式运行：
>  *     docker run -d 镜像名（镜像id）
>    * 参数：-d 让容器在后台运行，创建守护式容器 
>* 交互式运行：
>  *     docker run -it 镜像名 /bin/bash
>    * 参数： 
>      * -i 让容器的标准输入保持打开，交互模式下可通过创建的终端来输入命令
>      * -t 让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上
>      * /bin/bash 在容器中打开一个shell终端
>
>##### 查看容器状态
>    *     docker ps –a
>
>##### 启动/重启容器
>    *     docker container start  容器ID
>    *     docker container restart 容器ID
>      * 将一个运行中的容器停止再启动
>
>##### 进入容器
>*     docker exec -it 容器号 /bin/bash
>* 参数： 
>  * -i 让容器的标准输入保持打开，交互模式下可通过创建的终端来输入命令
>  * -t 让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上
>  * /bin/bash 在容器中打开一个shell终端
>
>##### 退出容器
>* exit  退出容器的时候，可能会导致容器停止
>* 先ctrl + p  再ctrl + q  ，这种方式退出容器，就不会停止
>
>##### 停止容器
>*     docker [container] stop 容器号
>
>##### 删除容器
>*     docker  [container] rm 容器号

>### 数据卷
>#### 数据卷特点
>* 数据卷 是一个可供一个或多个容器使用的特殊目录，特性有：
>  * （1） 数据卷可以在容器之间共享和重用
>  * （2） 对数据卷的修改会立马生效
>  * （3） 对数据卷的更新，不会影响镜像
>  * （4）数据卷被设计用来持久化数据的，它的生命周期独立于容器
>
>#### 数据卷的操作
>##### 创建数据卷
>*     docker volume create 数据卷名
>
>##### 查看数据卷
>* 列出所有数据卷
>  *     docker volume ls
>* 查看单个数据卷详细信息
>  *     docker volume inspect 数据卷名
>##### 删除数据卷
>  *     docker volume rm 数据卷名
>##### 清理无主数据卷
>  *     docker volume prune
>##### 挂载数据卷
>  *     docker run -d -p 宿主机端口：容器内端口 -v 数据卷名：容器内路径 镜像名（id）
>
>### 部署应用
>#### 使用docker部署
>* **网站部署思路**：
>    * 1、一个网站的运行一般需要的基础服务
>      * web服务
>      * 数据库服务
>      * 网页服务
>      * 解决办法：使用lamp镜像
>    * 2、端口映射：-p 主机端口:容器端口  
>      * 解决问题：如何从主机跳转进入容器
>    * 3、路径映射：-v 主机路径:容器发布路径 
>      *主机路径：就是放网站内容的地方（目录/路径/文件夹）
>      * 容器发布路径：镜像作者定义好的，不可更改
>      * 主机路径和容器发布路径都必须是绝对路径（从根目录开始的路径,以/开头）
>      * 解决：多个容器之间共享网站内容
>    * 4、数据卷的相关操作：数据卷就是一个目录，数据卷的名称就是这个目录的别称
>    * 5、用路径映射的方式发布网站的思路（六个步骤）
>      * a) 确定要使用的镜像 ，lamp或者lnmp之类的，或者主机部署数据库，web服务，网页服务的镜像
>      * b) 准备镜像，从docker仓库搜索下载，或者本地导入
>      * c) 拿到网站内容（开发成果），放到响应的目录（数据卷）下
>      * d) 得到容器的端口号以及容器的网站发布路径
>      * e) 运行镜像，生成容器
>      * f) 验证网站是否成功：打开浏览器：127.0.0.1：主机端口号/主页名称
>    * 6、经典错误：端口被占用（解决：换一个没有使用的端口）
>#### 使用docker-composer部署
>* 概念：
>    * Compose 是用于定义和运行多容器 Docker 应用程序的工具，以项目的方式来组织多个镜像完成一组业务功能实现并简化部署，有两个重要的概念：
>    * （1）服务(service):一个应用的容器，实际上可以包括若干运行相同镜像的容器实例。
>    * （2）项目(project):由一组关联的应用容器组成的一个完整业务单元，在docker-compose.yml中定义
>* **使用方式**：
>  * 获取项目的yml文件
>  * 执行
>  *     docker-compose logs
>  * 查看yml文件内容，了解项目使用的镜像，及可能设置的账号密码
>  * 根据实际环境修改yml文件。重点关注：
>    * MySQL root 用户的密码 (MYSQL_ROOT_PASSWORD and DB_ROOT_PASSWD)
>    * 持久化存储数据的相关 volumes 目录 (volumes)
>  * 运行yml文件
>    *     docker-compose up -d
>    * -d 以守护进程模式启动
>    * 会自动下载了需要的镜像以及启动一系列容器
>  * 查看已启动的容器
>    *     docker ps -a
>  * 打开应用，看部署是否成功
>  * 配置修改
>  * 修改了配置，例如改了端口号，可以重新运行docker-compose up –d，会自动停了原有容器新起容器，但是数据库和缓存都会保持原有的，因此原有的内容不会丢失。 
