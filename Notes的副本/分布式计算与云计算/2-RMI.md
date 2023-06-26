# 实验二：RMI 应用





<center>
  姓名：王磊
</center>





<center>
  学号：2020211538
</center>





<center>
  提交日期：2023-06
</center>





## Contents

[toc]

## 实验内容

1. 设计并实现⼀个简单的RMI应⽤程序，该应⽤程序包含⼀个远程接⼝和⼀个远程对象。 
远程接⼝ Calculator 包含两个⽅法 add 和 subtract ，⽤于实现加法和减法操作。远程 
对象 CalculatorImpl 实现了上述远程接⼝中的两个⽅法，并且通过RMI⽅式向客户端提 
供服务。
2. 设计并实现⼀个HTTP服务器应⽤程序，该应⽤程序能够接收客户端的HTTP请求，并且 
根据请求的内容调⽤RMI应⽤程序中的远程对象，并且将结果返回给客户端。
3. 设计并实现⼀个HTTP客户机应⽤程序，该应⽤程序能够向HTTP服务器发送HTTP请求， 
并且接收服务器返回的结果。
4. 可⾃⾏添加部分功能，包括但不限于：异常处理、⽂件传输、注册登录等，视情况加 
分。

## 实验环境

1. 实验平台：MacOS13 Ventura
2. 编程语言：Java——JDK18
3. IDE：IntelliJ IDEA 2021 (Ultimate Edition)



## 编码设计

### Java 类

#### Calculator 远程接口

代码如下：

```java
public interface Calculator extends Remote {
    int add(int a, int b) throws RemoteException;
    int subtract(int a, int b) throws RemoteException;
}
```

功能：包含 add 和 substract 方法用于实现加法和减法操作

注：接口声明满足两点要求：

1. 扩展 Remote 接口
2. 方法抛出 RemoteException 异常

#### 





### 执行程序





## 运行结果及分析





## 主要问题及解决方法







## 源程序清单