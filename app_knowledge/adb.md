# Android测试工具ADB

## 介绍
>### adb组成
>* adb客户端
>    * 提供用户一个输入命令的界面
>* adb服务端
>   *  维护电脑与手机的关系
>* adbd 守护进程
>   * 用于接受和执行命令
 
## adb 使用
>### adb安装和环境配置
>   * 解压platform-tools的压缩包，放在你自己的相应目录（英文）下 C:\platform-tools
>   * 把platform-tools中的adb.exe文件的路径加入全局变量path中  C:\platform-tools
>   * 安装模拟器
>   * 模拟器安装的位置，也要加入全局变量path中  D:\Program Files\Nox\bin
>   * 两个目录中的adb.exe 版本匹配（1.0.36比较稳定）
>   * 启动模拟器
>   * 启动adb客户端（进入命令提示符窗口）
>   * 确认是否连接成功 adb devices

>### adb常用命令
>* adb version 查看版本
>* adb help 查看帮助
>* adb root 获得root权限
>* adb start-server 启动adb服务器
>* adb kill-server 停止adb服务器
>* **adb pull 手机文件路径 电脑目录** 
>   * 从手机上取文件
>   * 不要放到根目录
>* **adb push 电脑目录 手机路径** 
>   * 电脑上文件放到手机上
>* adb devices
>   * -l 显示设备详情
>   * 显示 device 已连接
>   * 其他->重装模拟器
>* adb -s <设备序列号> 多台设备时指定设备执行命令
>* **adb [ -option ] install apk路径 安装apk到设备**
>   * -s 设备序列码  多台设备时指定设备安装，（序列码：ip地址 + 端口号）
>* **adb [ -option ] uninstall apk包名称**
>* 包名称：APP的唯一标识
>   * 查看方法：
>   * 1、在AndroidManifest.xml文件中查看 peckage 属性
>   * 2、利用aapt.exe
>   * 3、win:  adb shell dumpsys window | findstr mCurrentFocus 查看当前活动的信息
>      *  linux:  adb shell dumpsys window | grep mCurrentFocus 
						
>### adb shell 
>#### 进入android系统再执行命令
>#### adb shell + 命令，直接执行命令
>* adb shell screencap -p /sdcard/screen.png 屏幕截图
>* adb shell screenrecord 地址  录屏
>   * ctrl + c 停止

>##### adb shell am 系列(am 代表activity manager 活动管理)
>* start 包名/activity名 启动应用
>* force-stop 包名  停止应用

>##### adb shell pm 系列（package manager 包管理）
>* clear 包名 清除应用数据与缓存
>* enable 包名 启用某个应用
>* disable 包名 禁用某个应用
>* list features 列出手机硬件信息

>##### adb shell input 系列
>* adb shell input keyevent 编号 - 模拟手机常用按键  ，编号-->百度搜索：android keycode
>* adb shell input swipe x1 y1 x2 y2 - 手机滑动屏幕，adb shell wm size
>* adb shell input text 内容 - 在控件中输入信息
>* adb shell input tap x y - 在某个坐标点击，坐标查看：夜神模拟器：开发者选项---->打开“显示点按操作反馈”，“指针位置”


>### adb logcat 查看系统日志缓冲区信息
>* adb logcat -v time >C:\Users\z\Desktop\log.txt
>   * 解析：-- "-v"选项 : 设置日志的输出格式, 注意只能设置一项;以时间的格式将log输出
>* adb logcat -s System.out
>   * 输出指定标签内容 : 
>   * -- "-s"选项 : 设置默认的过滤器, 如 我们想要输出 "System.out" 标签的信息, 就可以使用 adb logcat -s System.out 命令
>* adb logcat -c 
>   * 清空日志缓存信息 : 使用 adb logcat -c 命令, 可以将之前的日志信息清空, 重新开始输出日志信息;
>* adb logcat -d
>   * 将缓存日志输出 : 使用 adb logcat -d 命令, 输出命令, 之后推出命令, 不会进行阻塞;
>* adb logcat -t 5
>   * 输出最近的日志 : 使用 adb logcat -t 5 命令, 可以输出最近的5行日志, 并且不会阻塞
>   * ctrl + c 退出
>* 日志级别：
>   * F: Fatal 崩溃
>   * W ：warning 警告
>   * I ：Info 系统信息
>   * V ：最低的，任何操作都会产生
>   * D ：调试
>   * E ：错误Error
>* logcat重定向
>   * （adb logcat  *:E > d:\logcat.txt）
>   * adb logcat > 地址
>   * 1、执行重定向命令
>   * 2、运行测试APP
>   * 3、ctrl + c 停止
>   * 4、查看日志
>* 按日志等级过滤输出
>   * adb logcat  *:W > d:\logcat.txt  只输出等级大于W的日志
>* 按关键字过滤
>   * windows下搜索日志:
>      *  adb shell "logcat |grep 'ActivityManager: Displayed com.atstudy.ando/.ui.MainActivity'"
>   * adb logcat | find "not found"  > d:\logcat.txt 
>   * 可以和按日志等级联用，adb logcat | find "not found" *:W > d:\logcat.txt 
>   * 常用关键字：exception(APP异常),crash(APP崩溃),anr(APP无响应或卡死)


>### 手机测试命令：
>* adb shell dumpsys battery 查看电池电量
>* adb shell dumpsys wifi 查看无线网络信息
>* adb shell dumpsys telephony.registry 查看电话相关信息 
>   * mCallState，
>      *  0：表示待机状态，
>      *  1：表示来电尚未接听状态，
>      *  2：表示电话占线 
>   * mServiceState，
>      *  0：表示正常使用状态，
>      *  1：表示电话没有连接到任何电信运营网络，
>      *  2：表示电话只能拨打紧急呼叫号码，
>      *  3：表示电话已关机。
>* adb bugreport 查看手机启动过程日志以及启动后系统状态
>* adb shell cat /proc/cpuinfo 查看cpu相关信息
>* adb shell cat /proc/meminfo 查看内存相关信息
>* adb shell cat /system/build.prop 查看手机信息
>* adb shell pm list packages 查看所有包列表信息
>   * list packages [ -option ] 查看所有包信息
>   * -3 第三方
>   * -1 系统应用
>
>* APP内模拟操作

>### monkey工具
>* 随机模拟：
>   * adb shell monkey -p 包名 --throttle 时间间隔 -s 种子值 -v -v -v 操作次数
>       * 会模拟人的操作，随机做各种行为
>   * 获取当前应用包名
>       *     adb shell dumpsys window | findstr "mCurrentFocus"
>* **指定动作：**
>  *     adb shell monkey -p com.tal.kaoyan --pct-touch 100 --throttle 500 -s 888 -v -v -v 100
>    * -p，指明要测试的app，如果不写-p参数是指针对手机整机来进行测试。
>    * --throttle，指明每次操作的间隔，一般设置为500，对应500ms（半秒），模拟人的正常速度。如果设置为更小的值，模拟人的快速点击，属于性能测试中压力测试。
>    * -s，通过一个数字来生成随机测试的操作序列（点击->滑屏->点击->点击->滑屏）。
>    * -v，用于设置monkey日志的详细程度，3个-v表示最详细。
>    * 可以添加多个动作，动作后带比例，总比例为100
>    * 最后跟动作次数
>* <img src="/app_knowledge/adb_monkey.png">
>* **输出日志到文件**
>    * 例如:
>        *     adb shell monkey -p com.taobao.taobao --throttle 1000 -v 50 >C:\Users\Desktop\monkeylog\log1