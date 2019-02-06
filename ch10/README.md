## 1.10 多进程服务器端

### 10.1 进程概念及应用

根据之前学到的内容，我们可以构建按序向第一个客户端到第一百个客户端提供服务的服务器端，不过这样第一百个的客户端需要很长时间的等待后才能得到服务，所以我们需要多并发。

#### 10.1.1 并发服务器端的实现方法

使服务器能同时向所有发起请求的客户端提供服务。而且网络程序中数据通信的时间比CPU运算时间占比更大，因此，向多个客户端提供服务是一种有效利用CPU的方式。

实现方法与模型
- 多进程服务器： 通过创建多个进程提供服务
- 多路复用服务器： 通过捆绑并统一管理I/O 对象提供服务
- 多路线服务器: 通过生成与客户端等量的线程提供服务

#### 10.1.2 理解进程(Process)

进程: 占用内存空间的正在运行的程序

#### 10.1.3 进程ID

所有进程都会从操作系统分配到ID，'进程ID'，其值为大于2的整数，1要分配给操作系统启动后的(用于协助操作系统)首个进程，因此用户进程无法得到ID值1。

通过ps指令可以查看当前运行的所有进程，该指令同时列出了PID(进程ID)，另外，通过指定a和u参数也能列出所有进程的详细信息

```
ps au
```
![](https://s2.ax1x.com/2019/02/06/kYHjeJ.png)

#### 10.1.4 通过调用fork函数创建进程

用于创建多进程服务器端的fork函数

```c++
#include <unistd.h>
// 成功时返回进程ID，失败时返回-1
pid_t fork(void);
```

fork函数将创建调用的进程副本，也就是说并非根据完全不同的程序来创建进程，而是复制正在运行的，调用fork函数的进程。另外，两个进程都经执行fork函数调用后的语句(准确来说是在fork函数返回后)。因为是同一个进程，复制相同的内存空间，之后的程序流要根据fork函数的返回值加以区分，即利用fork函数如下特点区分程序执行流程
- 父程序: fork函数返回子程序Id
- 子程序：fork函数返回0
