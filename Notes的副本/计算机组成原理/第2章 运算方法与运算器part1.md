## 数据的表示方法及其机器存储

### 数据的表示方法

#### 数据

1. 数值数据
   * 定点格式：数值范围有限，处理简单
   * 浮点格式：数值范围很大，处理比较复杂
2. 符号数据
   * 西文字符：ASCII码表示
   * 汉字：汉字输入码、汉字内码、汉字字模码

#### 选择数据的表示方法需要考虑的因素

1. 要表示的数的类型
2. 可能需要的数据范围
3. 数据精度
4. 数据存储和处理需要的硬件代价

#### 定点数的表示方法

1. 定点表示：小数点的位置固定不变
2. 通常在设计机器时即制定好小数点的位置
   * 小数点固定在最左边：定点纯小数
   * 小数点固定在最左边：定点纯整数
3. 符号也用数值表示

![1](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/1.png)

当n固定，小数点的位置也固定时，定点数可表示的数的范围也是固定的

##### 优点

表示方法简单，便与运算

##### 缺点

表示的数范围有限，要表示很大或很小的数需要很多比特

##### 大数量级数据的表示方法

1. 定点计算机的简介表示：在运算前按照一定固定比例缩放，在运算后再恢复
2. 用幂的方法运算

#### 浮点数的表示方法

1. 任意进制数N：$N=m\times R^e$

   * m：浮点数的尾数（纯小数）

   * e：阶，比例因子的指数，称为浮点数的指数（整数）

   * R：比例因子的基数（常数），在二进制机器中通常规定R为2,8或16

![2](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/2.png)

#### 十进制数串的表示方法

1. 字符表示，每位占一字节
2. 压缩十进制数串表示，每两位占一字节

#### 数的机器码表示

1. 数据表示：数值和符号均数值化

2. 数据运算：将符号位当做数值位统一参与运算

3. 真值和机器码
   * 真值：日常使用的正负号加绝对值表示数大小的原值
   * 机器码（机器数）：计算机中使用的表示数的形式，通常将数的符号位和数值位一起编码
   * 常用的定点数机器码：原码、补码反码、移码
   
4. 原码：在数值前加一个符号位，0表示正数，1表示负数，不存在小数点
   1. 优点：简单直观，便于转化
   2. 缺点：加减运算复杂
   
5. 补码：使加减操作统一，正负数表示和运算统一
   * 概念与取模运算
   
     ![3](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/3.png)
   
   * 定点小数
   
     ![4](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/4.png)
   
6. 反码：正数的反码为其本身，负数的反码等于把其相反数的各个二进制位取反

7. 已知反码求补码
   
   ![5](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/5.png)
   
8. 已知原码求补码![6](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/6.png)
   
   
   
9. 已知补码求原码
   
   ![7](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/7.png)

10. 移码（增码）表示法

    * 移码通常用于表示阶
    * 为简化操作，让所有阶码变为正数，对所有阶码加上一个固定的正常数（偏置常数）
      * 通常选择偏置常数的值为最负阶的绝对值
      * 若用 n 位整数表示阶，则偏置常数为 $2^n$
    * 对定点整数定义$[x]_{yi}=x+2^n$

11. 机器码的表示范围

    ![image-20220330161153020](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220330161153020.png)

 #### 机器码的比较

* 解决负数在机器中的表示与运算问题
* 若为正数，原码反码补码都与真值相同
* 最高位都可以看做符号位
  * 原码反码补码‘0’表示正号，‘1’表示负号
  * 移码‘1’表示正号，‘0’表示负号
* 移码与补码尾数相同，符号相反
* 0 有唯一的补码和移码
* 0 有两种原码和反码
* 补码、反码的符号位运算时可以当做数值，原码的符号位要单独处理

## 定点加减法运算

### 机器码的存储和运算

1. 原码定点数加减运算
   * 同号相加异号相减（绝对值大的数减绝对值小的数）
2. 补码加法
   * 两个加数的补码直接求和得的补码
   * $[x+y]_补=[x]_补+[y]_补$
   * 由此推出补码减法

### 溢出

* 发生溢出时运算结果已经错误，不能简单进行取模运算
* 溢出检测方法
  1. 先决条件：$|x+y|<1$
  2. 原理：两正数相加得负数，两负数相加得正数
  3. 单符号位法，逻辑表达式：$V=C_f\bigoplus C_0$
     1. $C_f$：符号位产生的进位
     2. $C_0$：最高有效位产生的进位
  4. ### 双符号位（变形补码）
     
     * ![image-20220402162551042](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220402162551042.png)
     * 任何小于 1 的正数，符号位为 00
     * 任何大于-1 的负数，符号位为 11
     * 结果的符号位出现 01 或 10 表示发生溢出
     * 最高符号位永远表示结果的正确符号
* 无符号数没有溢出的概念，超出最大值的进位直接丢弃
* 强制类型转换时，机器码并不改变，改变的是对机器码的解释方式

### 运算器的实现

1. 一位二进制全加器

   * ![image-20220402164825254](/Users/wangpaopaopao/Library/Application%20Support/typora-user-images/image-20220402164825254.png)
   * 三个输入两个输出
   * 两级三个与非门，两个异或门
   * 延迟时间：从输入数据信号有效到输出数据信号有效的时间

2. 串行加法器

3. 串行进位并行加法器

4. n 位串行（行波）进位并行补码加减法器

5. 进位产生和传递函数

   1. 并行加法器的进位链的基本逻辑关系：$C_{i+1}=A_i B_i+(A\bigoplus B_i)C_i$

   2. 串行改并行（先行进位）

      * 令$G_i=A_i B_i$为进位产生函数（本位进位）

      * $P_i=A_i\bigoplus B_i$为进位传递函数（进位条件）
      * $P_iC_i$为传送进位（条件进位）
      * 只有  $A_i=B_i=1$时，本位才向高位进位
      * 只有   $A_i\neq B_i$时，低一位进位将向更高位传递
      * 传送进位和本地进位不可能同时为 1
      * 根据公式，所有进位均可以从$C_0$直接求得

6. 4 位组内先行进位（超前进位）部件（CLA）

   * 延迟时间：2T

7. 16 位单级分组先行进位并行补码加法器

   1. 进位产生部件
   2. 进位逻辑<img src="https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220403144714429.png" alt="image-20220403144714429"  />
   3. 求和部件
   4. 小组内是并行，小组外是串行

8. 多级分组先行进位方式

   1. 小组内为先行进位
   2. 同一大组内的各个小组之间也采用并行进位
   3. 一个加法器有一个或多个大组
      * 若有多个大组，各大组之间既可以采用串行进位也可以采用并行进位
   4. 组间先行进位逻辑（BCLA）
      * ![image-20220403145944494](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220403145944494.png)
      * 实例：74182

9. 进位选择加法器





## 逻辑运算

1. 逻辑运算仅在无符号数之间进行
2. 逻辑运算与算术运算的区别
   1. 只在对应位之间进行
   2. 各位之间无进位/借位关系

## 多功能算数/逻辑运算单元

ALU：Arithmetic Logic Unit

### 4 位多功能 ALU——74181

1. 算术运算：4 位先行进位
2. 输出：$C_{n+4},P,G$：支持组间串行进位或组间先行进位
3. ![image-20220403153649045](https://raw.githubusercontent.com/wangpaopao-lei/pic/master/image-20220403153649045.png)

## 小结

1. 数据的表示方法及其及其存储
   * 便于存储，便于运算
2. 算术运算和逻辑与那孙的运算方法
3. 运算器的组成（实现）
   * 性能与成本的平衡

## 重点

1. 数值数据的表示方法
   * 机器数的表示范围、精度、特点
   * 定点数的机器码各种码制之间的转换
2. 定点算数运算的实现
   * 溢出的概念和检测方法
   * 加减法的统一
   * 先行进位
   * 运算器的硬件复杂度和性能
   * 定点运算器的组成和结构

