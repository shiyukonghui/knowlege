# Web 协议基础知识

>## Web协议
>* 协议：就是一种规范

>### OSI：Open System Interconnect 模型
>* **物理层（数据单位bit）**
>      * 0 ->低电平；1->高电平
>* **链路层**
>     * 交换机
>     * 传输数据帧
>     * 数据检错及重发
>* **网络层**
>      * 路由器
>      * 传输数据包
>      * 特点：寻找由起点路由器到终点路由器的最短路由路径
>      * 管的是IP地址
>* **传输层**
>    * 网卡：
>    * 管理端口
>    * 提供终端到终端的可靠连接
>* **会话层**
>* **表示层**
>* **应用层**
>* 会话层，表示层，应用层，在IP协议中统合成 应用层
>
> <img src="/img/TPC_IP.png"/>

>### TCP/IP模型
>#### 网络访问层
>* 以太网设备

>#### Internet层
>* IP协议：
>     * 代表设备：路由器
>  * IPV4 ：32位
>    * 可以用10进制表示
>    * 192.168.1.1 = 1100 0000.1010 1000.0000 0001.0000 0001
>  * IPv6：128位
>    * 16进制

>#### 传输层
>* 代表设备：本机

>##### TCP协议
>* 特点：
>   * 是一种面向连接的、可靠的、基于字节流的全双工的传输层通信协议；TCP补充了Internet协议，它定义了用于识别Internet上系统的IP地址，主要确保不同节点之间的端到端数据传输。
>* 协议内容：
>
>   * <img src="/img/TCP.png"/>

###### TCP通信过程
>* 浏览器输入域名
>  * DNS域名服务器解析得到Ip
>    * ping 域名服务器
>* 浏览器通过ip连接网站
>  * 三次握手
>    * 浏览器主动发起
>
>* 三次握手：
>  * 浏览器发送TCP包给服务器
>    * 同步信号：SYN = 1 
>      * 即头部绿色框内容
>    * 身份信号：Seq = X(0)（可以设置为随机数）
>      * 即头部中的确认号(红色框)
>      * 示例:
>  * <img src="/img/tcp_1.png"/>
>
>    * 服务器回复确认信息
>         * 同步信号：SYN = 1，ACK = 1
>         * 身份信号：Seq = Y(0)（可以设置为随机数），ack = X+1
>         * 示例：											
>  * <img src="/img/tcp_2.png"/> 

>    * 客户端发送确认信号
>      * 同步信号：ACK = 1
>      * 身份信号：Seq = x + 1 ，ack = Y +1
>      * 示例:
>  * <img src="/img/tcp_3.png"/>

>    * 传输数据
>    * 关闭连接
>
>  * 四次挥手
>    * 浏览器主动发起
>      * 客户端发送信号给服务器
>        * 同步信号：FIN = 1, ACK = 1
>        * 身份信号：Seq = X,ack = Z
>        * Z也是随机数，从经验看 z = y,协议规定中Z,Y可以不一样
>      * 服务端发送信号给客户端
>        * 同意挥手 
>      * 服务端再次发送信号给客户端
>        * 确认数据传输结束
>      * 客户端确认结束

###### 数据包分析
>* 抓包的作用：
>  * 从功能测试的角度检查传输数据的正确性
>    * （分析缺陷的4个方法：看界面，查数据库，看日志，抓包）
>  * 可以判断缺陷发生在客户端还是服务端
>  * 可以了解传输的数据是否加密
>  * 可以了解协议的内容，辅助接口，性能测试
> 
>* 抓包工具：
>  * wireshark(适用于网络层，Internet层，传输层) 
>    * 使用方法：
>      * 打开网页，保持2分钟以上
>      * 过滤规则：
>        * ip过滤：
>        * ip.addr ==ip地址  回车
>        * 关系符
>        * && 与符号
>        * || 或符号
>        * TCP过滤:
>        * tcp.port =端口号
>        * tcp.dstport 目的地端口 
>        * tcp.srcport 源端口
>        * UDP
> 
>* 抓包
>  * 抓包三次握手示例：
>
>      * 抓包四次挥手示例：
>
>      * 包传输的层次查看：
>
>      * 由上至下分别是：网络访问层（物理层（传输的是bits），链路层），internet层，传输层，应用层
>

>###### TCP 的流量控制算法
>*  TCP的流量控制是基于窗口机制实现的：
>   *  在建立连接时， 发送方和接收方都会建立一个缓存区，在两端进行通信时，数据包头部会有一个窗口字段，标识了接收端剩余的缓存空间。发送方根据窗口字段的值去判断发送数据的大小，从而避免了缓存溢出。
>*  TCP的拥塞控制算法包含了：
>   *  慢启动
>      *  慢启动指的是发送数据的量从较低的起始值，如一个报文段慢慢指数增长
>   *  拥塞避免
>      *  拥塞避免是指当拥塞的窗口小于阈值时，又指数增长降低为线性增长
>   *  快速重传
>      *  快速重传是指超过三次重复确认即视为传输失败，立即重传
>   *  快速恢复
>      *  快速恢复是指发生快速重传后，立刻减低窗口阈值，并进行拥塞避免的线性增长算法，避免因为拥塞阻碍了重传

>##### UDP协议:
>* 特点：面向非连接的，且不对传送数据包进行可靠性保证，适合于一次传输少量数据，UDP传输的可靠性由应用层负责。
					
>#### 应用层
>##### HTTP协议：
>* 特点：是一种请求-应答式的协议，客户端发送的每次请求都需要服务器回送响应，在请求结束后，会主动释放连接，客户端需要不断向服务器发起请求来保持在线状态。
>* http默认端口：80
>* https默认端口：443
>* 请求与响应
>  * 都分为：头部信息（headers）与身体信息（body）

>###### 请求
>* GET:
>      * 主要是数据获取
>      * 提交简单数据
>      * URL？参数键值对1 & 键值对2
>* POST:
>      * 主要是内容提交
>      * 会话详细
>      * 网格视图
>        * 能看到用户名和密码（是否加密，->加密测试）
>* 区别：
>      * POST请求方法比GET请求方法更安全：因为GET方法的参数和值会直接显示在URL地址，所以不安全；
>      * 而POST方法的参数和值是写在请求报文中然后上传至服务器的，不会裸露在URL地址中
>      * POST请求方法用于新增、修改和删除数据资源，而GET请求方法一般只用于数据资源的获取和查看
>      * POST请求方法“入参”（即：写参数和值）时的请求报文的文件大小“几乎”没有限制；而GET请求方法在入参时有一定的内容大小限制，一般是1MB
>      * GET产生一个TCP数据包；POST产生两个TCP数据包。对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。
>* put：
>      * 如果两个请求相同，后一个请求会把第一个请求覆盖掉。（所以PUT用来改资源）Post请求：后一个请求不会把第一个请求覆盖掉。（所以Post用来增资源）
>* delect：
>      * 对这个资源的删操作。但要注意：客户端无法保证删除操作一定会被执行，因为HTTP规范允许服务器在不通知客户端的情况下撤销请求。（即客户端没有权限删除）
>* HEAD方法：
>      * 与GET方法的行为很类似，但服务器在响应中只返回实体的主体部分。这就允许客户端在未获取实际资源的情况下，对资源的首部进行检查，使用HEAD，我们可以更高效的完成以下工作：
>      * 在不获取资源的情况下，了解资源的一些信息，比如资源类型；
>      * 通过查看响应中的状态码，可以确定资源是否存在；
>      * 通过查看首部，测试资源是否被修改；
>* TRACE方法：
>      * 会在目的服务器端发起一个“回环”诊断，我们都知道，客户端在发起一个请求时，这个请求可能要穿过防火墙、代理、网关、或者其它的一些应用程序。这中间的每个节点都可能会修改原始的HTTP请求，TRACE方法允许客户端在最终将请求发送服务器时，它变成了什么样子。由于有一个“回环”诊断，在请求最终到达服务器时，服务器会弹回一条TRACE响应，并在响应主体中携带它收到的原始请求报文的最终模样。这样客户端就可以查看HTTP请求报文在发送的途中，是否被修改过了。

> ###### 响应：Response
>* 正常响应
>   * 1xx
>      *  100
>         *  表示信息正在处理
>      *  101 ：websocket连接
>   * 2xx ：处理信息成功
>      *  200
>         *  表示服务器处理成功
>   * 3xx ：(重定向)表示要完成请求，需要进一步操作。通常这些状态代码用来重定向
>      *  301，永久性重定向，表示资源已被分配了新的 URL
>      *  302，临时性重定向，表示资源临时被分配了新的 URL
>      *  303，表示资源存在另一个URL，用GET方法获取资源
>      *  304，(未修改)自从上次请求后，请求网页未修改过。服务器返回此响应时，不会返回网页内容
>* 异常响应
>   *  4xx ：请求错误
>      *  400 请求消息中如果没有Host头域会报告一个错误（400 Bad Request）：语法错误
>      *  401 表示发送的请求需要有通过http认证的认证信息
>      *  403 服务器拒绝请求
>      *  404 服务器找不到请求的页面
>      *  409（Conflict）表示请求的资源与资源的当前状态发生冲突；
>      *  410（Gone）表示服务器上的某个资源被永久性的删除。
>   *  5xx：服务端错误
>      *  500 
>         *  服务器遇到错误，无法完成请求
>      *  503
>         *  服务器处于停机维护或超负载，无法处理请求
>* cookie
>      * 服务器保存在用户计算机上的一些资料，服务器用来识别用户
>* session
>      * 会话，客户端和服务端之间的的会话
>
>###### 版本特点
>* http 1.1
>   * 长连接
>      * HTTP1.1中默认开启Connection： keep-alive，一定程度上弥补了HTTP1.0每次请求都要创建连接的缺点。一次可以处理多个请求和响应
>      * 短连接和长连接（1.1版本）
>      * 长连接规定了一次可以处理多个请求
>      * 例如：长连接可以传输多张
>      * connection属性为：keep_alive
>      * 短连接用完就释放
>      * 短连接一次连接开始至停止只能传输一张图片
>   * 缓存处理
>      *  更多的缓存控制策略例如Entity tag，If-Unmodified-Since, If-Match, If-None-Match等更多可供选择的缓存头来控制缓存策略
>   * 带宽优化
>      *  在请求头引入了range头域，它允许只请求资源的某个部分，即返回码是206（Partial Content），这样就方便了开发者自由的选择以便于充分利用带宽和连接。
>   * 错误通知
>      *  新增了24个错误状态响应码，如409（Conflict）表示请求的资源与资源的当前状态发生冲突；410（Gone）表示服务器上的某个资源被永久性的删除。
>   * Host头处理
>      *  请求消息中如果没有Host头域会报告一个错误（400 Bad Request）
>   * 缺点：
>      *  HTTP1.1尽管减少了TCP连接的消耗，但是本身仍然是串行执行。若干个请求排队串行化单线程处理，后面的请求等待前面请求的返回才能获得执行机会，一旦有某请求超时等，后续请求只能被阻塞，毫无办法，也就是人们常说的线头阻塞
>* http 2.0 
>   *  二进制分帧
>      *  在应用层和传输层间增加二进制分帧层
>   *  多路复用
>      *  根据request的 id将request再归属到各自不同的服务端请求里面，即单个连接上同时进行多个业务单元数据的传输。
>      *  建立双向字节流，帧头部包含所属流 ID，帧可以乱序发送，数据流可设优先级和依赖。从而实现一个 TCP 会话上进行任意数量的HTTP请求，真正的并行传输。
>   *  头部压缩
>      *  压缩算法编码原来纯文本发送的请求头，通讯双方各自缓存一份头部元数据表，避免传输重复头HTTP2.0可以压缩头部的大小，并且避免了重复的传输，可以大大降低延迟
>   *  服务端推送（服务端可主动向客户端推送资源，无需客户端请求）
>      *  服务端推送能把客户端所需要的资源伴随着index.html一起发送到客户端，省去了客户端重复请求的步骤。正因为没有发起请求，建立连接等操作，所以静态资源通过服务端推送的方式可以极大地提升速度。
>   *  请求优先级
>* http 3.0
>   * 当一个 TCP 会丢包时，整个会话都要等待重传，后面数据都被阻塞。这是由于 TCP 本身的局限性导致的。HTTP3.0 基于 UDP 协议，解决 TCP 的局限性。
>   * 0-RTT
>      *  缓存当前会话上下文，下次恢复会话时，只需要将之前缓存传递给服务器，验证通过，即可传输数据。
>   * 多路复用
>      *  一个会话的多个流间不存在依赖，丢包只需要重发包，不需要重传整个连接。
>   * 更好的移动端表现
>      *  移动端 IP 经常变化，影响 TCP 传输，HTTP3.0 通过 ID 识别连接，只要 ID 不变，就能快速连接。
>   * 加密认证的根文
>      *  TCP 协议头没有加密和认证，HTTP3.0 的包中几乎所有报文都要经过认证，主体经过加密，有效防窃听，注入和篡改
>   * 向前纠错机制
>     *   每个包还包含其他数据包的数据，少量丢包可通过其他包的冗余数据直接组装而无需重传。数据发送上限降低，但有效减少了丢包重传所需时间。
> 
>##### Https
> 
>##### Websocket
>###### 特点
>* WebSocket 是独立的、创建在 **TCP** 上的协议。
>* Websocket 通过HTTP/1.1 协议的**101状态码**进行握手。
>* 在WebSocket API中，浏览器和服务器**只需要完成一次握手**，两者之间就直接可以创**建持久性的连接**，并进行**双向**数据传输。
>* WebSocket使得客户端和服务器之间的数据交换变得更加简单，允许**服务端主动向客户端推送**数据。
>
>###### websocket的原理
>* websocket约定了一个通信的规范，通过一个握手的机制，客户端和服务器之间能建立一个类似tcp的连接，从而方便它们之间的通信
>* 在websocket出现之前，web交互一般是基于http协议的短连接或者长连接
>* websocket是一种全新的协议，不属于http无状态协议，协议名为"ws"
>* 建立连接过程：
>  * 首先，客户端发起http请求，经过3次握手后，建立起TCP连接；http请求里存放WebSocket支持的版本号等信息，如：Upgrade、Connection、WebSocket-Version等；
>  * 然后，服务器收到客户端的握手请求后，同样采用HTTP协议回馈数据；
>  * 最后，客户端收到连接成功的消息后，开始借助于TCP传输信道进行全双工通信。
> 
> 
>##### 数据包分析
>* 应用层抓包工具：Fiddler，浏览器开发者工具
>   * Fiddler使用方法： 
>   * 封装和解封装（打包和解包）
>   * 单工，双工，半双工
>      * 单工：广播电台（单向通信）
>      * 半双工：对讲机（可以通信，但数据不能同时来往）
>      * 双工：手机（可以同时通信）


