># html : 超文本标记语音
>* 工具：IDE（集成开发环境）

>## 单标签
>* \<meta charset="UTF-8"> 
>  * charset 为标签属性

>## 双标签
>* \<h> \</h> 标题
>      * h1 -> h6 大小不同
>* \<p> \</p> 段落
>* \<strong>\</strong>  加粗
>* i 斜体
>* 可以使用\<font> \</font>对文本进行修饰
>  * 属性：
>    * size
>    * color
>* \<a href=""></a> 链接
>  * "#"代表 链接到自身
>  * 也可以放置图片路径
>    * \<img src="图片地址" alt="">
>  * 使用相对路径

>### 表格绘制
>* table
>  * th 表头
>  * tr 行
>  * td 列
>  * colspan 合并左右单元格
>  * rowspan 合并上下单元格

>### 列表，下拉菜单
>#### ul 无序列表(无序号)
>  * li 标签（list）
> 
>#### ol 有序列表
>    * li 标签（list）
>
>#### select 下拉框
>    * option
>      * 可以通过 selected 设置默认选项

>### 表单提交
>#### form
>  * action 提交的网页
>  * method 提交的类型
>    * post 提交内容放在请求体中，可以提交多种类型
>    * get 提交的内容跟在链接后面，长度有限
>
> #### input 输入框
>    * type可以设置text , password，submit等类型
>    * submit 提交功能
>
>#### frame 布局
>    * 删除body标签
>    * 使用frameset
>      * rows 参数分割行
>        * 可以使用百分比来划分布局大小
>        * 可以使用像素来指定
>        * 可以使用 \* 号来分配，比如 "\*，\*，\*"  则分成3份，每一个占比 1*/3*
>      * cols 参数分割列
>    * frame 标签
>      * src 参数放网页地址

>## Pycharm操作快捷方式
>* tab键的使用可以自动补全标签
>* li*3 + tab可以快速生成3个标签
>* ctrl + /  快速注释 或者 取消注释

>## 特殊符号
>* \&nbsp; 空格
>* \<br/>  换行
>* \&lt;  左箭头，小于
>* \&gt;  右箭头，大于
>* \&amp;  显示&

