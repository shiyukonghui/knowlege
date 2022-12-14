# 操作系统实战
>* 《操作系统实战45天》的学习记录
## 一切的开端“Hello World”

>1. #include "stdio.h"
>2. int main(int argc, char const *argv[])
>3. {
>4. printf("Hello World!\n");
>5. return 0;
>6. }

### 程序编译过程
>* **使用命令：**
>  *     gcc HelloWorld.c -o HelloWorld 或者 gcc ./HelloWorld.c -o ./HelloWorld ，
>* 就可以编译这段代码。其实，GCC 只是完成编译工作的驱动程序，它会根据编译流程分别调用预处理
>* 程序、编译程序、汇编程序、链接程序来完成具体工作
>* <img src="/img/c编译.jpg">
>* 过程可分为：
>      * 源文件生成预处理文件： gcc -E HelloWorld.c -o HelloWorld.i
>      * 预处理文件生成编译文件： gcc -S HelloWorld.i -o HelloWorld.s
>      * 编译文件生成汇编文件： gcc -c HelloWorld.s -o HelloWorld.o
>      * 汇编文件生成可执行文件：gcc HelloWorld.o -o HelloWorld
>      * 源文件生成可执行文件：gcc HelloWorld.c -o HelloWorld
>      * Linux系统运行可执行文件：./HelloWorld


### 程序装载执行
>* 图灵机是一个抽象的模型，它是这样的：有一条无限长的纸带，纸带上有无限个小格子，
>* 小格子中写有相关的信息，纸带上有一个读头，读头能根据纸带小格子里的信息做相关的操作并能来回移动。
>* <img src="/img/图灵机.png">
#### 冯诺依曼结构
>* 具有如下功能：
>      * 把程序和数据装入到计算机中；
>      * 必须具有长期记住程序、数据的中间结果及最终运算结果；
>      * 完成各种算术、逻辑运算和数据传送等数据加工处理；
>      * 根据需要控制程序走向，并能根据指令控制机器的各部件协调操作；
>      * 能够按照要求将处理的数据结果显示给用户。
>* 计算机五大组件：
>      * 装载数据和程序的输入设备；
>      * 记住程序和数据的存储器；
>      * 完成数据加工处理的运算器；
>      * 控制程序执行的控制器；
>      * 显示处理结果的输出设备。
#### Hello World 的执行
>* 通过 gcc -c -S HelloWorld 得到（只能得到其汇编代码，而不能得到二进制数据）。
>* 我们用 objdump -d HelloWorld 程序，得到 /lesson01/HelloWorld.dump，
>* 其中有很多库代码（只需关注 main 函数相关的代码）
>* <img src="/img/helloworld汇编.png">
>* 以上图中，分成四列：
>      * 第一列为地址；
>      * 第二列为十六进制，表示真正装入机器中的代码数据；
>      * 第三列是对应的汇编代码；
>      * 第四列是相关代码的注释。这是 x86_64 体系的代码，由此可以看出 x86 CPU 是变长指令集。
>* <img src="/img/hello状态.png">
#### 回顾
>* 以上，对应图中的伪代码可以明白：
>* 现代电子计算机正是通过内存中的信息（指令和数据）做出相应的操作，并通过内存地址的变化，
>* 达到程序读取数据，控制程序流程（顺序、跳转对应该图灵机的读头来回移动）的功能。
>* 这和图灵机的核心思想相比，没有根本性的变化。只要配合一些 I/O 设备，让用户输入
>* 并显示计算结果给用户，就是一台现代意义的电子计算机。到这里，我们理清了程序运行
>* 的所有细节和原理。还有一点，你可能有点疑惑，即 printf 对应的 puts 函数，到底
>* 做了什么？这正是后面的要探索的

#### 思考
>* 思考题为了实现 C 语言中函数的调用和返回功能，CPU 实现了函数调用和返回指令，
>* 即上图汇编代码中的“call”，“ret”指令，请你思考一下：call 和 ret 指令在逻辑上
>* 执行的操作是怎样的呢？

>* call和ret其实是一对相反指令，调用call时会将当前IP入栈，即push IP，
>* 然后执行跳转即jmp，而ret也是将栈中的IP推出写入IP寄存器，即pop IP。

>* call 指令会把当前的 PC(CS:IP) 寄存器里的下一条指令的地址压栈，然后进行JMP跳转指令；
>* ret 指令则把 call 调用时压入的 PC 寄存器里的下一条指令出栈，更新到 PC 寄存器中