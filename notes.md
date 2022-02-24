## **6.S081** ##

make sure you do the **assigned reading** for the labs, read the relevant files through, consult the **documentation** (the RISC-V manuals etc. are on the [reference page](https://pdos.csail.mit.edu/6.828/2021/reference.html)) before you write any code. Only when you have a **firm grasp** of the assignment and solution, then start coding.  When you start coding, **implement your solution in small steps** (the assignments often suggest how to break the problem down in smaller steps) and test whether each steps works before proceeding to the next one.

**目的**

* 理解操作系统是如何被设计和实现出来的
* 获得实际的建立系统软件的动手经验
* 扩展一个简单的操作系统（xv6）
* 学习一个硬件是如何工作的（Risc-V）



**我将要做**

* 为网络栈建立驱动，该网络栈能够在真实网络上发包
* 重新设计内存分配器，从而可以跨多核间进行扩展
* 实现fork功能并通过一个copy-on-write的优化使其高效



**OS的目标**

* 抽象
  * 为了便于端口化及方便性而隐藏硬件细节
  * 一定不能够降低高品质
  * 必须能够支持广泛的应用软件

* 多路复用
  * 允许多个应用软件共享硬件
  * 独立掌控bug并提供安全性保护
  * 共享以允许合作



**OS抽象(O/S内核主要提供的服务）**

* 进程（执行中的程序）
* 内存分配
* 文件描述符

* 文件名和目录
* 访问控制和配额
* 其他：用户态，进程间通信，网络套接字，时间等等



**用户态和内核间接口**

* 主要的系统调用

  * 举例：

  ```
  fd = open("out",1);
  len = write(fd,"hello\n",6);
  pid = fork();
  ```

​		以上例子看起来像是函数的调用，但其实不是



xv6运行在Risc-V上，例如6.004

利用Qemu运行xv6（仿真）

接口很简单，就是整形变量和I/O缓冲器



**pipe()**

* 一个文件描述符（FD）指代一个管道，同时也是一个文件

* pipe()系统调用创建两个FD
  * 从第一个FD读取
  * 向第二个FD写入

内核为每一个管道保留一个缓冲器



A few pointer common idioms are in particular worth remembering:

- If `int *p = (int*)100`, then     `(int)p + 1` and `(int)(p + 1)`    are different numbers: the first is `101` but    the second is `104`.    When adding an integer to a pointer, as in the second case,    the integer is implicitly multiplied by the size of the object    the pointer points to.
- `p[i]` is defined to be the same as `*(p+i)`, referring to the i'th object in the memory pointed to by p. The above rule for addition helps this definition work when the objects are larger than one byte.
-  `&p[i]` is the same as `(p+i)`, yielding the address of the i'th object in the memory pointed to by p.

分清一个内存地址是指针形式还是整形变量的形式，从而确保是否需要乘object的大小



**Debug**

* 在script里make qemu,所有的控制台输出记录在一个文件里，便于查找问题，记得推出script

* 



**架构**

* 芯片架构，操作系统之下的东西，一个架构可以安装linux，windows等多种操作系统。

* loongnix架构也可以运行ubuntu、windows ？？

​	答：理论上可以，但windows源码不开源，无法对接。



**git**

**分布式**的版本控制系统，旨在高效地处理从小型到超大型项目的所有内容



提交到仓库：拷贝一份，自己git init



查找问题：搜索关键字，让浏览器匹配问题



**convention**

是一个小的整形变量，这个整形变量代表一个进程可以读出或写入的内核管理的obj。
