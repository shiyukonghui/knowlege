# Selenium 的使用
>## 注意事项
>* selenium控制浏览器长时间运行后,占用内存不断增多

>## 页面元素定位：
>### WebDriver API 操作
>#### 获取页面源码
>*     br.page_source
>#### 获取页面标题
>*     br.title
>#### 前进
>*     br.forward()
>#### 后退
>*     br.back()
>#### 刷新
>*     br.refresh()
>#### 最大化
>*     maximize_window()
		
>## 定位单个元素：
>### find_element系列：
>* find_element_by_id( id属性值 )：
>   * id定位，根据元素的id属性值定位，最为方便且唯一，但有可能不存在，也可能动态生成
>* find_element_by_name( name 属性值 )：
>   * name定位，根据元素的name属性值定位，但通常不唯一
>* find_element_by_class_name( class 属性的取值 )：
>   * class定位，根据元素的class属性值定位，但可能受JS影响动态变化
>   * 如果class属性中有空格的，代表这个标签有多个class属性，选择其中一个才能定位
>* find_element_by_tag_name( 标签属性值 )：
>   * tag name定位，根据元素的标签名定位，但很少唯一
>* find_element_by_link_text(‘xx’)：
>   * 根据链接标签内的普通文本进行定位。
>   * 只能针对链接元素（a元素）
>* find_element_by_partial_link_text(‘xx’)：
>   * partial（坡舍）
>   * 根据链接文本的部分文本内容进行定位。
>* find_element_by_xpath(‘xx’)：
>   * 根据元素的xpath表达式来完成定位，可以准确定位任何元素，但需要熟练掌握xpath语法
>* Xpath定位方法：
>   * Xpath是一种在XML文档中定位元素的语言：
>   * XML：可扩展的标记语言：格式：<标签名 属性1=“值1”>普通文本</标签名>
>      *  使用绝对路径的方式来定位：
>         *  指的是从网页的HTML代码结构的最外层一层层的写到需要被定位的页面元素为止。绝对路径起始于/，每一层都被/所分割。
>            *  示例：/html/body/div[2]/form/input[3]
>         *  可以用中括号选择分支，div[2]代表的是当前层级下的第二个div标签；
>         *   一般情况下较少使用绝对路径的方式做定位，原因在于绝对路径的表达式一般太长，不便于后期的代码维护，代码的微小改变就可能导致这个路径失效，从而无法完成元素定位。
>      *  使用相对路径的方式来定位
>         *  不是从根目录写起，而是从网页文本的任意目录开始写。
>         *  相对路径起始于//，//所表示的含义是“任意目录下”：
>            *  示例：//input[@id=’kw’]
>         *  在xpath里，属性以@开头，所选取的属性可以是任意属性，只要其有利于标识这个元素即可
>         *  推荐使用相对路径结合属性的这种xpath表达式，它往往更简洁更易于维护
>         *  可以使用逻辑运算符and来连接多个属性进行标识。
>            *  示例：//input[@xx=’aa’ and @yy=’bb’]
>         *  有时候一个元素它本身没有可以唯一标识它的属性，这时我们可以找它的上层或者上上层，然后再往下写。
>            *  示例：//input[@xx=’aa’]/y
>         *  可以选择使用浏览器的开发者工具获取xpath表达式：
>            *  f12——》使用元素拾取器在你想要定位的元素上面点一下，代码区域就会高亮显示出该元素所对
>            *  应的代码，再在该代码上右击，选择“复制”——》选择“xpath”——》到需要使用xpath表达式的地方黏贴就能看到这个表达式：
>            *  //*[@id="kw"]
>         *  根据普通文本定位
>            * find_element_by_xpath("//a[text()='退出']")
>            * find_element_by_xpath("//em[text()='登录']")
>            * 部分文本定位：
>              * find_element_by_xpath("//em[contains(@text,'登录')]")
>* 共同特点：
>   * xpath里，属性以@开头
>   * 路径分层：xpath里使用的是  /
>   * 注意冒号的层次性
>   * xpath中通过class属性来定位的话，这种“pn vm”形式有空格的，包含了两个属性值，要写全，类似：//input[@class=’pn vm’]
>      *  跟find_element_by_class_name 使用方式冲突，这里要写成//input[@class=’pn’]或者//input[@class=’vm’]
>   * 如何验证自己的xpath语句正确性：
>      *  火狐浏览器中可以输入xpath语句验证是否正确
>* find_element_by_css_selector(‘xx’)：
>   * 根据元素的css选择器来完成定位，可以准确定位任何元素，但需要熟练掌握css选择器
>   * CSS（西赛思）定位方法：
>      *  通过CSS选择器来定位：
>         *  class类选择器 .
>            *  .xxx
>               *  选择class属性为xxx的元素
>         *  id选择器 #
>            *  #xxx
>               *  选择id属性为xxx的元素
>         *  标签元素选择器
>            *  直接写标签名即可
>            *  如input标签css直接写input就好
>               *  abc
>               *  选择标签名为abc的元素
>         *  属性选择器
>            *  [title = "bbb"]
>               *  选择title属性为bbb的元素
>         *  选择器可以组合
>            *  标签名在前
>            *  后面跟属性（可以多个）
>      *  既可以使用相对路径也可以使用绝对路径
>         *  相对路径（常用）
>         *  绝对路径
>         *  路径使用 > 分层
>   * 共同特点：
>      *  如果页面上有匹配的元素，它会返回找到的页面元素，返回值的数据类型是WebElement对象；
>      *  如果页面上没有匹配的元素，则该方法会报错，引发一个NoSuchElementExcept异常；
>      *  如果页面上有多个匹配的元素，通常情况下，只返回找到的第一个匹配的元素（注意：做元素定位的时候，尽量不要去使用有可能取多个但是定位第一个的情况，因为不同软件的实现可能不同，这个状态不确定，应该尽量写准确，能定位到某一个元素。）

>## 定位多个元素：
>* find_elements_by_xxx系列：
>   * 类似单个元素的8种方法
>   * 特点：
>      * 它返回的是一个包含WebElement对象的列表；
>      * 如果没有找到任何元素，不会报错，返回一个空的列表，长度为0

>## 操作页面元素：
>* get_attribute(“属性名”)
>   * 获取元素中某个属性的值
>* Select类
>   * 变量e =  Select(获取元素)
>   * e. select_by_value("值")
>      *  通过下拉选项option的value属性来选
>   * e.select_by_visible_text(“w文本”)
>      *  通过下拉选项option的文本来选
>   * e.select_by_index(序号)
>      *  通过下拉选项option的序号来选
>   * e.options
>      *  获取所以option的列表
>   * text
>      * 获取某个标签元素的普通文本信息

>## 页面切换：
>* 窗口句柄：
>   * 句柄：标识每个窗口的标识号
>   * window_handles返回的是一个窗口句柄的列表，这些窗口句柄都有它所对应的编号
>      *  第一个打开的窗口编号是0，最后一个打开的窗口编号是1，倒数第二个打开的编号是2，以此类推
>      *  两个页面：0 1
>      *  三个页面：0 2 1
>      *  四个页面：0 3 2 1
>* 通过窗口句柄切换
>   * current_window_handle
>      *  获取当前操作页面的句柄（不是显示的，切换句柄后才能操作其他页）
>   * 通过循环句柄列表来切换页面
>   * switch_to.window(句柄)
>* 通过窗口索引切换
>   * fire_browser.switch_to.window(fire_browser.window_handles[1])

>## 打开新标签页
>* 定义js语句：
>   * js1 = " window.open('网址')"
>   * execute_script(js1)

>## 框架切换
>* 标签：frameset ,frame,iframe
>* frame框架切换：
>   * 方法一：
>      *  先定位到frame元素
>      *  switch_to.frame(元素)切换到frame页面
>   * 方法二：
>      *  如果frame有id 或者name属性
>      *  switch_to.frame（属性名）直接切换
>   * 方法三：
>      *  通过frame框架索引切换
>      *  通过find_elements…定位到一组frame框架标签，再通过索引切换
>   * 怎么判断是否在frame里
>      *  检查源码->查看css路径是否有frame
>   * 从框架中切出：
>      * switch_to.default_content()

>## 处理弹窗：
>* WebAPI操作：
>   * 确认弹窗
>      *  switch_to.alert().accept()
>   * 取消弹窗
>      *  switch_to.alert().dismiss()
>   * 获取弹窗上的文本
>      *  switch_to.alert().text
>   * 重新看到弹窗
>      * 刷新：refresh()

>## 鼠标操作：
>* 定位元素
>* 鼠标操作
>   * 导入模块：
>      *  from selenium.webdriver import ActionChains
>   * ActionChains(br).move_to_element(e).perform()
>      *  move_to_element(e) 鼠标悬停
>      *  perform() 执行所有存储在ActionChains()类中的行为，做最终的提交
>   * ActionChains(br).double_click(e).perform() 双击
>   * ActionChains(br).context_click(e).perform() 右击

>## 键盘操作：
>* 定位元素
>* 模拟键盘输入：
>   * 导入模块
>      *      from selenium.webdriver.common.keys import Keys
>* 输入：
>   *      send_keys()
>   * 示例：
>      *      send_keys(Keys.BACK_SPACE)
>      *  #全选（ctrl+a）
>         *      e.send_keys(Keys.CONTROL,'a')
>      *  #剪切
>         *      e.send_keys(Keys.CONTROL,'x')
>      *  #黏贴
>         *      e.send_keys(Keys.CONTROL,'v')
>      *  #回车
>         *     e.send_keys(Keys.ENTER)

>## JS调用
>*     js1 = document.getElemmentById('id属性值') 
>*     execute_script(js1)
>  * 运行js代码，同样可以定位元素

>## 截图：
>* 提交BUG需要
>   *     get_screenshot_as_file("文件名")

>## 元素等待：
>* 因为网页加载要时间，网络原因等
>### 强制等待
>* sleep(时间，单位秒)
>* python自带
>* Sleep()可用于避免因元素未加载出来而定位失败的情况。
>* 缺点;
>   * 如果指定的时间过长，即使元素加载好了，还是会继续等待，这样会浪费时间

>### 隐式等待：
>* Implicitly_wait(等待时间)
>* selenium提供
>* 一般在打开浏览器后进行声明，等待网页加载完成再继续下一步，超时报错
>* 优点：
>   * 等待时间内加载完成就结束等待
>   * 只需要声明一次，在浏览器关闭前都有效
>* 缺点：
>   * 程序会一直等待整个页面加载完成才会执行下一步，有时候想要定位的元素早就加载好了，
>   * 但是由于别的页面元素没加载好，仍会等到整个页面加载完成才能执行下一步。

>### 显示等待：
>* WebDriverWait(浏览器变量，等待时间，查询时间).until(条件)
>* 显式等待只针对指定的元素生效，不再是针对所有的页面元素，
>* 超时报错
>* 优点：
>   * 可以根据需要定位的元素来设置显式等待，无需等待页面完全加载，节省了大量因加载无关紧要的页面元素而浪费的时间
>* 跟sleep()一样，在需要时使用
>* 使用方法
>* 效率：显示等待 > 隐式等待 > 强制等待
