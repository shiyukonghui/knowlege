#  Appium的使用
>* 结构：请求-响应式
>   * cient
>      *  执行脚本
>   * server
>      *  解析脚本，转换为手机能执行的命令
>      *  返回执行结果
>   * phone
>      * 进行APP操作，返回操作结果

## 脚本准备步骤
>1. 启动Appium服务端
>2. 启动Appium Inspector
>   * 配置参数，启动
>      *  配置Remote Path ：/wd/hub
>      *  配置Remote Host：服务端地址
>      *  配置Remote Port : 服务端端口号
>      *  在Desired Capabilities中输入Capabilities的json格式数据
>      *  点击Start Session开始
>   * 在inspector中可以定位元素
>      *  注意：运行python代码后,inspector自动过期，需要重新start_session
>3. 编写脚本操作
>   * 使用python语言
>   * 导包
>   * 配置参数变量
>      * Capabilities的设置
>        * platformName：手机操作系统 
>        * platformVersion：手机操作系统版本
>        * deviceName：手机或模拟器类型
>        * app：安装包的绝对路径
>        * appPackage：运行的android包名
>        * appActivity：要启动的android activity
>        * noReset：设置在当前session下不会重置应用的状态，
>          * 默认为false---->每次都重新安装app,重置之前的操作和个人数据
>          * true---->不会重新安装app
>          * 重置：重新安装

### APP管理
>* app安装
>  * 配置了安装路径后，自动安装
>     * 验证是否成功安装：
>       *     driver.is_app_installed（包名）
>     * 成功返回True
>     * 失败返回False
>* app卸载
>  *     driver.remove_app(包名)
>  * 同样可以用is_app_installed(包名)来检查卸载是否成功

### 元素定位
>* appium
>* uiautomatorviewer（Android SDK自带，跟appium冲突，不能同时使用）
>   * 在terminal中运行：uiautomatorviewer  打开
> 
>#### Appium Inspector
>* id定位
>  * resourse_id唯一，则可以称为id,不唯一，则可以定位一组元素
>      * 一组元素的话，通过软件上方search按钮寻找到元素的索引
>* class name定位
>  * 跟html不同
>  * 如果class不唯一，同样定位一组元素
>* xpath定位
>  * 相对路径
>    * 可以使用模糊查询
>    * //省略了路径，可以代表任意路径
>    * * 通配符，匹配任意符号
>  * 部分文本属性匹配：
>    * //*\[contains(@text,"取消")]
>  * 文本匹配：
>    * //*[@text="取消"]
>  * 对于一组元素，xpath路径只有一处不同，即列表索引，也可用这个来定位 
>  * 绝对路径
>
>#### android原生方法定位
>* android语言: java
>  * 原生方法：也可以称为底层方法，之前的Appium的id,class定位都是以原生方法为基础
>  * 注意：第二个单词首字母要大写
>##### 使用text文本属性定位：
>  *     driver.find_element(AppiumBy.ANDROID_UIAUTOMATOR,'text("取消")')
>    * AppiumBy.ANDROID_UIAUTOMATOR创建一个对象
>    * 对象调用text()
>      * 括号里必须是双引号(java字符串是双引号)
>##### 部分文本定位
>*     driver.find_element(AppiumBy.ANDROID_UIAUTOMATOR,'textContains("取消")')
>  * 通过resourceId定位
>     *     driver.find_element(AppiumBy.ANDROID_UIAUTOMATOR,'resource_Id("id")')
>  * 通过className定位
>     *     driver.find_element(AppiumBy.ANDROID_UIAUTOMATOR,'className("className")')
>##### 相对定位：
>  * 即元素不好定位的，可以翻阅层级关系来查找其父元素或者同级元素
>###### 父定位：
>* 先找到父元素，再定位到其子元素
>   * 方法1：原生方法
>     *     son=resourceId("com.tal.kaoyan:id/activity_register_parentlayout").childSelector(className("android.widget.ImageView"))'
>     *     image_btn = driver.find_element(AppiumBy.ANDROID_UIAUTOMATOR,son)
>   * 方法2:xpath方法
>     *     //*[@text="选项"]//[@text="取消"]
>###### 同级定位：
>* 先找到一个同级元素，找到它的父辈，再从父辈找到子元素
>* 方法1：原生方法
>  *     brother='resourceId("com.tal.kaoyan:id/activity_register_username_edittext").fromParent(className("android.widget.ImageView"))'
>  *     image_btn = driver.find_element_by_android_uiautomator(brother)
>* 方法2:xpath方法
>  *     //*[@text="更新"]/../[@text="取消"]
							
### 异常处理:
>* try:
>  * #要执行的操作
>* except:
>  * #异常处理
>* else:
>  * #无异常的处理
### 操作元素 
### 操作屏幕
>* 创建TouchAction对象，通过对象调用想要执行的手势，通过perform()执行动作
>* 导入包
>  *     from appium.webdriver.common.touch_action import TouchAction
>#### 手指轻敲屏幕操作
>* tap()方法
>  * 点击元素位置
>    *     el = find_element_by_id(…)
>    *     TouchAction(driver).tap(el).perform()
>  * 点击坐标，使用x,y坐标
>    *     TouchAction(driver).tap(x,y).perform()
>#### 手指按下操作
>* press()方法
>   * 元素定位
>      *     el = find_element_by_id(…)
>      *     TouchAction(driver).press(el).perform()
>   * 使用坐标
>      *     TouchAction(driver).press(x,y).perform()
>#### 模拟手指长按操作
>* long_press()方法,可以设置按压时间
>   * 元素定位
>      * el = find_element_by_id(…)
>      * TouchAction(driver).long_press(el,duration=5000).perform()
>   * 使用坐标
>      * TouchAction(driver).long_press(x,y,duration=5000).perform()								
>#### 滑动
>* 坐标滑动
>   *     driver.swipe(x1,y1,x2,y2)
>* 元素滑动
>1. 寻找元素
>2. 元素定位
>3. 滑动
>     *     driver.scroll(元素1，元素2)
>     *     driver.drag_and_drop(元素1,元素2)
>#### 多点连续滑动
>*     TouchAction(driver).press(坐标1).move_to(坐标2).move_to(坐标3).move_to(坐标4).release().perform()
>  * 绝对坐标
>  * 中间可以添加wait(时间)等待一个点的滑动完成

>#### 两点同时滑动
>1. 创建两个多点滑动
>  * TouchAction(driver).press(起点坐标).move_to(第二点坐标).release()
>2. 把两个多点滑动合在一起
>  * 创建MultiAction()对象
>  * 对象使用add(滑动1，滑动2)
>  * 对象使用perform()执行
#### 版本改变
>* 在 appium2.0 之前，在移动端设备上的触屏操作，单手指触屏和多手指触屏分别是由 TouchAction 类，Multiaction 类实现的。
>* 在 appium2.0 之后，这 2 个方法将会被舍弃。会报错误："\[Deprecated] 'TouchAction' action is deprecated. Please use W3C actions instead."
>##### w3c action
>* [官方文档](https://www.w3.org/TR/webdriver1/#actions)
>* 示例:长按元素3秒
>* **Appium2.0之前长按元素方法**:
>1. 导入TouchAction类库
>2. 定位要长按的元素
>3. 使用TouchAction的long_press方法，指定等待时间3s。（Note：此处wait单位为ms。）
>4. 长按之后要松开，所以用了一个click方法。
>* **Appium2.0之后长按元素方法**：
>1. 导入selenium的ActionChains类库
>2. 使用指针输入源pointer的鼠标操作PointerActions，如上图。
>3. 定位要长按的元素
>4. 使用click_and_hold方法按住元素并保持
>5. 使用pause方法指定按住的停顿时间2s（Note：单位为s）
>6. 使用release方法松开鼠标
>7. 最后使用perform方法执行以上操作。
### appium自动化测试
>* 自动化测试：用代码代替手工
>* 主要在回归测试阶段执行
>####  search原则
>* s--setup ---测试的预置条件，测试准备工作
>* e--execute---测试执行
>* a--analysis---测试分析
>* r--report---测试报告
>* c--clearup----teardown---测试的清理工作
>* h--help----document----测试总结文档
>#### 与手工测试区别
>* 并非所有的步骤都有验证，只验证关键点
>* 数据驱动---测试数据----参数
>* 设置还原点----清理工作
>* 使用unittest类
>   * 创建python单元测试文件
>   * 设置测试固件---setUp和tearDown
>   * 编写测试方法
>* 碰到的问题
>  * 自动化测试框架的优点
>     *  减少重复代码
>     *  方便地维护配置信息
>     *  业务单元的代码被执行多次，而且要适应不同的用例场景
>     *  用多组数据驱动自动化测试
>     *  直接得到测试用例是否通过的结果，而不是业务逻辑执行的结果
>     * 生成简洁美观的测试报告
