# 多核版大作业

进程：程序的执行活动
多线程：程序中多段代码同时运行
一个进程至少包括一个线程（主线程）
多个线程共享数据空间（代码段、数据段）

---
产生线程：
    将函数的程序结构从依赖调用改为自主运行，
    在main函数中添加创建子程序的语句
    设计互斥和同步方案
    确定子线程和其他线程间共享的数据（线程参数、全局变量）

## 创建线程 uintptr_t_beginthreadex
```
(void *security, //安全属性，默认为null
unsigned stack_size, //指定堆栈大小，若为0则与创建它的线程相同
unsigned (*start_address)(void*), //用函数名即可，这是指向函数的指针
void*arglist, //传给线程的参数指针，传多个参数时用结构体
unsigned initflag, //线程初始状态 0：立即运行  create_suspend 挂起
unsigned *thrdaddr) //线程id
```
成功就返回句柄，失败返回0

**创建线程的语句结构**
```
#include<windows.h> // 线程函数需要操作系统的函数
#include<process.h>// 线程函数头文件
#include<stdio.h> 
unsigned __stdcall getInput (void* pArguments); 
int main()
{ 
HANDLE hThread1;// 句柄
unsigned ThreadID=1; // 线程的标识号 
hThread1=(HANDLE) 
_beginthreadex(NULL, 0, getInput, NULL, 0, &ThreadID); // 创建一个getInput函数的线程 
printf(“main thread is running\n”); //主线程输出W
aitForSingleObject(hThread1, INFINITE); //等子线程结束
CloseHandle (hThread1); //无须控制线程时，则用该语句删除句柄。
}
```
**变为线程的函数结构**
```
unsigned __stdcall getInput (void* pArguments) // 线程函数的返回值是指定类型，形参也只能有一个 
{
printf("hThreadl is running\n"); //自定义的函数体
_endthreadex(0); // 结束线程 
return 0;
}
```


---

* 互斥
  * 
* 同步

---

**寄存器是核心私有的而内存是全局共享的**
将原有寄存器数量扩大（但并非简单的x2，而是将单个变量换成一维数组，一维数组换成二维数组）
内存不做修改