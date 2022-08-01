# Burp Suite 使用

>## Burp suite使用
>   * 代理功能
>   * 入侵功能
> 
>### 爬网功能
>* 需求:收集所有页面的输入框,等待扫描
>* 使用步骤:
>  * 关闭截包功能
>  * 页面过滤,只处理目标网站(设置白名单过滤出目标网站)
>  * 对目标网站,页面表单设置
>  * 主动爬网,获取所有页面
> 
>### 扫描功能(Scanner)
>* 模拟各种攻击漏洞,xss漏洞
>* 主动扫描
>    * options--->设置scan issues
>* 设置扫描漏洞类型
>  * options--->Active Sacnning Optimization
>  * 设置扫描的速度(不是越快越好)
>  * 设置精确度
>    *  尽量多报(不确定也报)
>    *  正常
>    *  尽量少报(不确定不报)
>  * Target 中右键Activitely scan this branch
>  * Scanner--->Lssue activity扫描报告
>  * 选择漏洞报告--->右键-->Report issue 产生报告
>  * 文件格式html
>
>* 解码功能
>* 比较器功能
>* 中继器和时序器
>   * 中继器:快速发出重复请求,可以在浏览器查看响应
>   * 时序器:分析一段值有没有规律
>     *  poor 有规律
>     *  excellent 优秀



