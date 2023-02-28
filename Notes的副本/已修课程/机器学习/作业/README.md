## 公式推导

![image-20221019004520992](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20221019004520992.png)

## Python 实现

- 使用 sigmoid 作为激活函数

```python
def sigmoid(x):
```

- 初始化单隐层反向传播神经网络

```python
    def __init__(self, input_nodes, hidden_nodes, output_nodes, learning_rate):
```

- 使用梯度下降方法训练神经网络

```python
    def train(self, inputs_list, targets_list):
```

- 使用训练好的模型预测测试集数据

```python
    def query(self, inputs_list):
```

## 不同隐藏节点数

实现方法标准：

- 隐藏节点数 300，学习率 0.3，迭代次数 1 次，数据标准化方法为将 [0,255][0,255] 区间映射到 [0,1][0,1] 区间

  ![img](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102103127082.png)

以下测试方法均在此基础上做改变，分别改变隐藏节点数、学习率、迭代次数和数据标准化方法。

- 隐藏节点数 10

  ![image-20211102102213505](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102102213505.png)

- 隐藏节点数 30

  ![image-20211102102355099](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102102355099.png)

- 隐藏节点数 100

  ![image-20211102102547988](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102102547988.png)

- 隐藏节点数 300

  ![image-20211102103127082](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102103127082.png)

- 隐藏节点数 1000

  ![image-20211102121025097](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102121025097.png)

由以上结果可以得出，隐藏节点数越多，神经网络准确度越高。但是由神经网络理论，我们知道这并不是正确的。首先，节点数增多，参数也就更多，神经网络模型会增大。其次，参数一味地增多，会导致神经网络过拟合，影响模型在测试集上的表现。

## 不同学习率

- 学习率为 0.01

  ![image-20211102105107208](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102105107208.png)

- 学习率为 0.1

  ![image-20211102105759076](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102105759076.png)

- 学习率为 1

  ![image-20211102110732893](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102110732893.png)

- 学习率为 10

  ![image-20211102120150423](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102120150423.png)

调整学习率，神经网络准确度会先随学习率升高而升高，之后会最学习率升高而降低。

## 不同迭代次数

- 迭代次数为 5

  ![image-20211102121941287](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102121941287.png)

- 迭代次数为 20

  ![image-20211102102020257](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102102020257.png)

- 迭代次数为 50

  ![image-20211102074207770](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102074207770.png)

  迭代次数越大，神经网络精确度越高。

## 改变数据标准化方法

- 将 [0,255][0,255] 区间映射到 [−1,1][−1,1] 区间

  ![image-20211102122230373](https://gitee.com/buptsg2019/picgo/raw/master/image-20211102122230373.png)

  与 [0,1][0,1] 区间相比准确度降低。