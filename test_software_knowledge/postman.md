# Postman
>## 优缺点
> ### 优点
>* 开源免费
>* 不断迭代产品，更新速度快，功能丰富
>* UI界面非常酷炫，科技感十足
>* 从企业级接口测试工作的角度来说，用户易用性更好
>* 不仅仅是测试人员喜欢使用，开发人员很多也是使用Postman来做开发人员之间的接口联调的
>* 现在接口文档流行使用Swagger写，Postman完美支持从Swagger接口文档中导入被测试接口，大幅度提升工作效率（也有不少专业人士不太喜欢和弃用这个功能）
>
> ### 缺点
>* 虽然新版本也支持接口性能测试，但几乎没人用，毕竟不是靠接口性能测试起家的，不专业
>* 功能太完整了反而导致菜单很庞大，而且很多功能不怎么实用
>* 做接口自动化测试时，需要先安装Newman环境才可以使用命令行模式启动并集成在“Jenkins”中
>* 如果英语不好，那么全英文的操作界面也算是一个缺点
> 

># postman使用
>* **先创建一个集合collections**
>* **创建文件夹分类**
>
>## 创建请求
>* 创建POST请求
>     * 数据保存在body中，以键值对的方式保存在json中
>* 创建GET请求
>   * 接口地址与参数以？隔开
>   * 参数键值对以&隔开

>## 管理环境变量
>* 点击右上角眼睛按钮
>* 上面是局部变量，当前项目
>* 下面是全局变量，其他项目也能用
>* 进入新建环境变量，输入变量名 及其代表的接口地址
>* 私有变量建好后，还要在右上角选择环境
>* 变量的使用：
>   *  {{变量名}}/接口路径（/参数）

>## 不同接口间的数据传递
>* 可以新建变量，保存后，选择令牌或其他参数，将其值右键set:变量名后可以快捷使用
>* 可以通过在Tests里写js代码将参数值添加到全局或者局部变量
>* 如果引用的变量名（但值不一定相同）在公共环境和私有环境中是相同的，那么读取的是私有变量中的那个

>## 断言
>*     pm.test
>  * 测试函数用来生成一个测试，可以输入测试标题（默认是Your test name），并在function函数内加入各种断言； 一个接口请求可以添加多个test测试函数。
>*     pm.expect
>  * 断言函数，用来生成各种断言，一个test测试函数里可以只有一个expect断言函数，也可以是多个； 如果是多个情况，所有断言函数全部通过才算测试成功，只要某一个断言函数失败，接口测试最终就失败。

## 持续集成的自动化测试
>### 何时使用数据驱动测试模式
> *  当接口请求的入参需要覆盖不同测试数据的时候，比如使用不同的账户名登录，虽然参数值会不同，但是参数名一直是username
> *  一般情况下我们不使用数据驱动技术，因为使用成本较高，如果需要测试三种不同的登录，其实写三个登录接口更快更简单
> *  但是当你遇到接口的某个入参的测试数据需要准备很多不同的数据时，数据驱动测试模式更高效以及接口在以后的可维护性更高
> *  基于Postman工具的特性，如果需要执行基于数据驱动的接口测试，该接口请单独在批量运行器中执行
> *  只有批量运行器(Runner)的运行模式才支持“数据驱动测试模式”
> *  数据驱动测试模式是一种思想，永远先考虑为什么要使用数据驱动技术，如果真的有必要用，必须先设计数据驱动文件
> *  Postman支持csv格式和json格式的数据驱动文件，csv格式的数据驱动文件在实战中有很多限制，所以弃用
> *  数据驱动文件中的每一组数据都是一条测试用例，只需设计需要改动的入参即可；参阅提前设计好的json格式的数据驱动文件"AtStudy_OKR_DataDriven_for_JSON"

>### python从批量创建json数据文件
>* 通过python读取json模板，从数据库读取数据填入模板

> ### newman 数据驱动
>*     newman run name.postman_collection.json -g 环境变量.poatman_globals.json -d 数据地址.json
>  * name是创建的集合collections 的名称。

### 从Swagger接口文档导入测试接口
> 等待完工
> ***

### Jenkins触发器
>* 设置定时执行
>* 构建中选择linux shell或者windows命令行：
>  * 触发newman 命令，实现自动化执行
>  * newman命令要以在jenkins系统中命令行能使用的为准
>* <img src="/img/jenkins_time.png"/>