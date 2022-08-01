# Postman 使用

>## postman使用
>   * **先创建一个集合collections**
>   * **创建文件夹分类**
>
>### 创建请求
>* 创建POST请求
>     * 数据保存在body中，以键值对的方式保存在json中
>* 创建GET请求
>   * 接口地址与参数以？隔开
>   * 参数键值对以&隔开
> 
>### 管理环境变量
>* 点击右上角眼睛按钮
>* 上面是局部变量，当前项目
>* 下面是全局变量，其他项目也能用
>* 进入新建环境变量，输入变量名 及其代表的接口地址
>* 私有变量建好后，还要在右上角选择环境
>* 变量的使用：
>   *  {{变量名}}/接口路径（/参数）
> 
>### 不同接口间的数据传递
>* 可以新建变量，保存后，选择令牌或其他参数，将其值右键set:变量名后可以快捷使用
>* 可以通过在Tests里写js代码将参数值添加到全局或者局部变量
>* 如果引用的变量名（但值不一定相同）在公共环境和私有环境中是相同的，那么读取的是私有变量中的那个
>
>### 断言：
>*     pm.test
>  * 测试函数用来生成一个测试，可以输入测试标题（默认是Your test name），并在function函数内加入各种断言； 一个接口请求可以添加多个test测试函数。
>*     pm.expect
>  * 断言函数，用来生成各种断言，一个test测试函数里可以只有一个expect断言函数，也可以是多个； 如果是多个情况，所有断言函数全部通过才算测试成功，只要某一个断言函数失败，接口测试最终就失败。
>### newman 数据驱动
>      *  newman run xxx.postman_collection.json -g 环境变量.poatman_globals.json -d 数据地址.json
>### Jenkins触发器
>      * 设置定时执行
>* <img src="/img/jenkins_time.png"/>