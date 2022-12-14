# Fiddler 使用

>## fiddler与APP测试
>* 步骤1:设置代理
>   * fiddler代理设置,设置代理端口,勾选Allow remote computers to connect,重启fiddler
>   * 模拟器代理设置,网络设置代理: 127.0.0.1 ;端口号设置为fiddler端口
>   * 终端命令:
>     *  adb reverse tcp:fiddler代理端口号 tcp:fiddler代理端口号
>     *  将模拟器的端口绑定到主机,实现连接
>* fiddler工具在移动app测试中主要涉及到以下部分：
>   * Inspector模块使用
>     * 可以比界面检查更多的数据，从而进行更细的测试。
>   * AutoResponder模块使用
>   * 模拟各种响应
>     * 将要模拟的请求拖到自动转发页面
>     * 勾选启用规则和unmatched requests passthrough
>     * 修改响应数据
>       *  右键编辑,切换到textview标签页,修改对应参数
>   * 抓取https数据
>   * 1、下载fiddler的cer格式证书；
>     *  进入tools-options菜单的https标签页，操作actions中的导出证书到桌面，导出的文件为cer文件。
>   * 2、利用openssl将cer格式证书转换为pem证书，并对pem证书重新命名
>     *  将cer格式证书转换成pem证书
>     *  查看转换出来的证书cacert.pem的hash值
>     *  根据得到的hash值:例如 269953fb 将cacert.pem的文件名修改为269953fb.0
>   * 3、将重新命名的证书放到手机的系统证书目录下。
>     * 利用adb push将269953fb.0文件拷贝到逍遥模拟器的/system/etc/security/cacerts/目录下。
>     * 如果提示只读，先运行adb remount后再运行以上命令，adb remount指以可写方式重新挂载。

>## fiddler与web测试
> 
>### 抓包场景
>* 测试软件时发现了bug，需要判断是前端问题还是后端问题，然后提交bug给对应的前端开发或者后端开发
>* 通过mock技术，模拟“假的”服务器接口响应数据，从而提高测试效率
>* 通过数据篡改技术（比如：Fiddler的断点功能）做一些安全性测试
>* 项目中要求做接口测试，但开发又懒得给接口文档（是一种极不规范、不负责任、害项目害公司的行为），无奈之下只能靠自己一边在前端操作软件，一边通过抓包工具抓数据并分析，“猜测出”有哪些接口、哪些接口参数、每个参数入参时的需求以及服务器响应内容等等
