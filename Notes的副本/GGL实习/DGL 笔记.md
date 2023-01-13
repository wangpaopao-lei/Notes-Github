# chapter1 graph

## 1.2 

---

![image-20220512145222809](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220512145222809.png)



点 ID：一维整型张量（节点张量）

DGL使用一个包含2个节点张量的元组 (𝑈,𝑉)(U,V) ，其中，用 (𝑈[𝑖],𝑉[𝑖])(U[i],V[i]) 指代一条 𝑈[𝑖]U[i] 到 𝑉[𝑖]V[i] 的边

### 图的建立

![image-20220512183758143](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220512183758143.png)

```python
import dgl
import torch as th

# 边 0->1, 0->2, 0->3, 1->3
u, v = th.tensor([0, 0, 0, 1]), th.tensor([1, 2, 3, 3])
g = dgl.graph((u, v))
print(g) # 图中节点的数量是DGL通过给定的图的边列表中最大的点ID推断所得出的
Graph(num_nodes=4, num_edges=4,
      ndata_schemes={}
      edata_schemes={})

# 获取节点的ID
print(g.nodes())
tensor([0, 1, 2, 3])
# 获取边的对应端点
print(g.edges())
(tensor([0, 0, 0, 1]), tensor([1, 2, 3, 3]))
# 获取边的对应端点和边ID
print(g.edges(form='all'))
(tensor([0, 0, 0, 1]), tensor([1, 2, 3, 3]), tensor([0, 1, 2, 3]))

# 如果具有最大ID的节点没有边，在创建图的时候，用户需要明确地指明节点的数量。
g = dgl.graph((u, v), num_nodes=8)
```

### 转为无向图

每条边添加反方向的边

```python
bg = dgl.to_bidirected(g)
```

### 尽量使用 32 位整数

```python
edges = th.tensor([2, 5, 3]), th.tensor([3, 5, 0])  # 边：2->3, 5->5, 3->0
g64 = dgl.graph(edges)  # DGL默认使用int64
print(g64.idtype)
torch.int64
g32 = dgl.graph(edges, idtype=th.int32)  # 使用int32构建图
g32.idtype
torch.int32
g64_2 = g32.long()  # 转换成int64
g64_2.idtype
torch.int64
g32_2 = g64.int()  # 转换成int32
g32_2.idtype
torch.int32
```

## 1.3

---

### BG

torch.ones(*sizes, out=**None**) → Tensor

返回一个全为1 的张量，形状由可变参数`sizes`定义。默认为 float32 类型

torch.randn(*sizes, out=**None**) → Tensor

返回一个张量，包含了从标准正态分布(均值为0，方差为 1，即高斯白噪声)中抽取一组随机数，形状由可变参数`sizes`定义。 参数:

- sizes (int...) – 整数序列，定义了输出形状
- out ([*Tensor*](http://pytorch.org/docs/tensors.html#torch.Tensor), *optinal*) - 结果张量

x = torch.tensor([5.5,3])

根据数据直接创建 Tensor

### 节点和边的特征

创建

g.ndata[' ']=

g.edata[' ']=

访问某节点特征

g.ndata['X']\[1]

访问某几条边的特征

g.edata['X']\[Tensor]

```python
import dgl
>>> import torch as th
>>> g = dgl.graph(([0, 0, 1, 5], [1, 2, 2, 0])) # 6个节点，4条边
>>> g
Graph(num_nodes=6, num_edges=4,
      ndata_schemes={}
      edata_schemes={})
>>> g.ndata['x'] = th.ones(g.num_nodes(), 3)               # 长度为3的节点特征
>>> g.edata['x'] = th.ones(g.num_edges(), dtype=th.int32)  # 标量整型特征
>>> g
Graph(num_nodes=6, num_edges=4,
      ndata_schemes={'x' : Scheme(shape=(3,), dtype=torch.float32)}
      edata_schemes={'x' : Scheme(shape=(,), dtype=torch.int32)})
>>> # 不同名称的特征可以具有不同形状
>>> g.ndata['y'] = th.randn(g.num_nodes(), 5)
>>> g.ndata['x'][1]                  # 获取节点1的特征
tensor([1., 1., 1.])
>>> g.edata['x'][th.tensor([0, 3])]  # 获取边0和3的特征
    tensor([1, 1], dtype=torch.int32)
```

1. 仅允许数值类型特征，可以是标量、向量、多维张量
2. 每个节点特征和边特征峰有唯一名字，但是点特征和边特征可以重名
3. ==通过张量分配创建特征时，DGL会将特征赋给图中的每个节点和每条边。该张量的第一维必须与图中节点或边的数量一致。 不能将特征赋给图中节点或边的子集==
4. 相同名称的特征必须具有相同的维度和数据类型

### 加权图

```python
# 边 0->1, 0->2, 0->3, 1->3
>>> edges = th.tensor([0, 0, 0, 1]), th.tensor([1, 2, 3, 3])
>>> weights = th.tensor([0.1, 0.6, 0.9, 0.7])  # 每条边的权重
>>> g = dgl.graph(edges)
>>> g.edata['w'] = weights  # 将其命名为 'w'
>>> g
Graph(num_nodes=4, num_edges=4,
      ndata_schemes={}
      edata_schemes={'w' : Scheme(shape=(,), dtype=torch.float32)})
```

## 1.5 异构图

---

一个异构图由一系列子图构成，一个子图对应一种关系，每个关系有一个字符串三元组定义（源节点类型，边类型，目标节点类型）

eg![image-20220514000322894](https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20220514000322894.png)

### 创建异构图

```python
import dgl
>>> import torch as th

>>> # 创建一个具有3种节点类型和3种边类型的异构图
>>> graph_data = {
...    ('drug', 'interacts', 'drug'): (th.tensor([0, 1]), th.tensor([1, 2])),
...    ('drug', 'interacts', 'gene'): (th.tensor([0, 1]), th.tensor([2, 3])),
...    ('drug', 'treats', 'disease'): (th.tensor([1]), th.tensor([2]))
... }
>>> g = dgl.heterograph(graph_data)
>>> g.ntypes
['disease', 'drug', 'gene']
>>> g.etypes
['interacts', 'interacts', 'treats']
>>> g.canonical_etypes
[('drug', 'interacts', 'drug'),
 ('drug', 'interacts', 'gene'),
 ('drug', 'treats', 'disease')]
```

同构图和二分图就是特殊的异构图

### 图的模式

metagraph()

 *metagraph* 中的一个节点 𝑢 对应于相关异构图中的一个节点类型。 *metagraph* 中的边 (𝑢,𝑣) 表示在相关异构图中存在从 𝑢 型节点到 𝑣 型节点的边

就是异构图的抽象和总结

### 获取信息的方法

引入多种节点和边类型后，==在调用特定类型信息时需要指定类型==

```python
# 获取图中所有节点的数量
>>> g.num_nodes()
10
>>> # 获取drug节点的数量
>>> g.num_nodes('drug')
3
>>> # 不同类型的节点有单独的ID。因此，没有指定节点类型就没有明确的返回值。
>>> g.nodes()
DGLError: Node type name must be specified if there are more than one node types.
>>> g.nodes('drug')
tensor([0, 1, 2])

# 设置/获取"drug"类型的节点的"hv"特征
g.nodes['drug'].data['hv'] = th.ones(3, 1)
g.nodes['drug'].data['hv']
tensor([[1.],
        [1.],
        [1.]])
# 设置/获取"treats"类型的边的"he"特征
g.edges['treats'].data['he'] = th.zeros(1, 1)
g.edges['treats'].data['he']
tensor([[0.]])
```

如果只有一种节点或边类型，不需要指定（同构图的方法）

### 从磁盘加载异构图

## 1.6

---

### 在 GPU 上使用图

g.device 查看图的位置

cuda_g=g.to('cuda:0') 将 g 传到 GPU 上，连同特征数据一起拷贝

### 使用 GPU 张量创建图

u,v=u.to('cuda:0'),v.to('cuda:0')

g=dgl.graph(u,v)



# chapter2 message passing

边上计算: 
$$
m_{e}^{(t+1)}=\phi\left(x_{v}^{(t)}, x_{u}^{(t)}, w_{e}^{(t)}\right),(u, v, e) \in \mathcal{E}
$$
$\phi$：消息函数，将边上特征和两端节点的特征结合来生成消息

点上计算: 
$$
x_{v}^{(t+1)}=\psi\left(x_{v}^{(t)}, \rho\left(\left\{m_{e}^{(t+1)}:(u, v, e) \in \mathcal{E}\right\}\right)\right)
$$
$\rho$：聚合函数，聚合节点接收到的消息

$\psi$：更新函数，结合聚合后的消息和节点本身的特征来更新节点的特征

## 2.1

---

### 消息函数 

接受一个参数 `edges`，这是一个 `EdgeBatch` 的实例， 在消息传递时，它被DGL在内部生成以表示一批边。 `edges` 有 `src`、 `dst` 和 `data` 共3个成员属性， 分别用于访问源节点、目标节点和边的特征。

- 要对源节点的 `hu` 特征和目标节点的 `hv` 特征求和， 然后将结果保存在边的 `he` 特征上，用户可以使用内置函数 `dgl.function.u_add_v('hu', 'hv', 'he')`。 而以下用户定义消息函数与此内置函数等价。
- ```python
  def message_func(edges):
     return {'he': edges.src['hu'] + edges.dst['hv']}



### 聚合函数 

接受一个参数 `nodes`，这是一个 `NodeBatch` 的实例， 在消息传递时，它被DGL在内部生成以表示一批节点。 `nodes` 的成员属性 `mailbox` 可以用来访问节点收到的消息。 一些最常见的聚合操作包括 `sum`、`max`、`min` 等。

- 聚合函数通常有两个参数，它们的类型都是字符串。一个用于指定 `mailbox` 中的字段名，一个用于指示目标节点特征的字段名， 例如， `dgl.function.sum('m', 'h')` 等价于如下所示的对接收到消息求和的用户定义函数：

  ```python
  import torch
  def reduce_func(nodes):
       return {'h': torch.sum(nodes.mailbox['m'], dim=1)}

### 更新函数 

接受一个如上所述的参数 `nodes`。此函数对 `聚合函数` 的聚合结果进行操作， 通常在消息传递的最后一步将其与节点的特征相结合，并将输出作为节点的新特征。

- 在不涉及消息传递的情况下，通过 [`apply_edges()`](https://docs.dgl.ai/generated/dgl.DGLGraph.apply_edges.html#dgl.DGLGraph.apply_edges) 单独调用逐边计算。 [`apply_edges()`](https://docs.dgl.ai/generated/dgl.DGLGraph.apply_edges.html#dgl.DGLGraph.apply_edges) 的参数是一个消息函数。并且在默认情况下，这个接口将更新所有的边。

- 例如：

  ```python
  import dgl.function as fn
  graph.apply_edges(fn.u_add_v('el', 'er', 'e'))
  ```

- update_all 是一个高级 API。参数是一个消息函数、一个聚合函数和一个更新函数（可选）

- 例如：

  ```python
  def update_all_example(graph):
      # 在graph.ndata['ft']中存储结果
      graph.update_all(fn.u_mul_e('ft', 'a', 'm'),
                       fn.sum('m', 'ft'))
      # 在update_all外调用更新函数
      final_ft = graph.ndata['ft'] * 2
      return final_ft

## 2.2

---

apply_edges()有时边上信息是高维的，会非常消耗内存，建议尽量减少特征维度。

拼接源节点和目标节点特征，如何用一个线性层（线性层输出维度较低）

### 直接实现

```python
import torch
import torch.nn as nn

linear = nn.Parameter(torch.FloatTensor(size=(node_feat_dim * 2, out_dim)))
def concat_message_function(edges):
     return {'cat_feat': torch.cat([edges.src.ndata['feat'], edges.dst.ndata['feat']], dim=1)}
g.apply_edges(concat_message_function)
g.edata['out'] = g.edata['cat_feat'] @ linear
```

### 高效实现

将线性操作分成两部分，分别用于源节点和目标节点，最后将两个相加

```python
import dgl.function as fn

linear_src = nn.Parameter(torch.FloatTensor(size=(node_feat_dim, out_dim)))
linear_dst = nn.Parameter(torch.FloatTensor(size=(node_feat_dim, out_dim)))
out_src = g.ndata['feat'] @ linear_src
out_dst = g.ndata['feat'] @ linear_dst
g.srcdata.update({'out_src': out_src})
g.dstdata.update({'out_dst': out_dst})
g.apply_edges(fn.u_add_v('out_src', 'out_dst', 'out'))
```

## 2.3

---

### 部分消息传递

先在囊括的节点编号创建子图，再在子图上 update_all()

```python
nid = [0, 2, 3, 6, 7, 9]
sg = g.subgraph(nid)
sg.update_all(message_func, reduce_func, apply_node_func)
```

## 2.5

---

### 异构图消息传递

在DGL中，对异构图进行消息传递的接口是 [`multi_update_all()`](https://docs.dgl.ai/generated/dgl.DGLGraph.multi_update_all.html#dgl.DGLGraph.multi_update_all)。 [`multi_update_all()`](https://docs.dgl.ai/generated/dgl.DGLGraph.multi_update_all.html#dgl.DGLGraph.multi_update_all) 接受一个字典。这个字典的每一个键值对里，键是一种关系， 值是这种关系对应 [`update_all()`](https://docs.dgl.ai/generated/dgl.DGLGraph.update_all.html#dgl.DGLGraph.update_all) 的参数。

==相当于对多个 update_all()的封装==

还接受一个字符串进行跨类型整合函数

```python
import dgl.function as fn

for c_etype in G.canonical_etypes:
    srctype, etype, dsttype = c_etype
    Wh = self.weight[etype](feat_dict[srctype])
    # 把它存在图中用来做消息传递
    G.nodes[srctype].data['Wh_%s' % etype] = Wh
    # 指定每个关系的消息传递函数：(message_func, reduce_func).
    # 注意结果保存在同一个目标特征“h”，说明聚合是逐类进行的。
    funcs[etype] = (fn.copy_u('Wh_%s' % etype, 'm'), fn.mean('m', 'h'))
# 将每个类型消息聚合的结果相加。
G.multi_update_all(funcs, 'sum')
# 返回更新过的节点特征字典
return {ntype : G.nodes[ntype].data['h'] for ntype in G.ntypes}
```

# chapter3 Building GNN modules

DGL NN模块，取决于后端使用的深度学习框架

==相同点==：构造的参数注册和前向传播函数中使用的张量操作和后端框架一样

==区别==：特有的消息传递机制

## 3.1

---

### 构造函数

1. 构造函数
2. 注册可学习参数或者子模块
3. 初始化参数

```python
import torch.nn as nn

from dgl.utils import expand_as_pair

class SAGEConv(nn.Module):
    def __init__(self,
                 in_feats,
                 out_feats,
                 aggregator_type,
                 bias=True,
                 norm=None,
                 activation=None):
        super(SAGEConv, self).__init__()

        self._in_src_feats, self._in_dst_feats = expand_as_pair(in_feats)
        self._out_feats = out_feats
        self._aggre_type = aggregator_type
        self.norm = norm
        self.activation = activation
```

1. 设置数据维度

   - 一般的模块：输入维度、输出维度、隐层维度
   - 图神经网络：源节点特征维度、目标节点特征维度

2. 聚合类型（如何聚合不同边上的信息

   ```python
   # 聚合类型：mean、pool、lstm、gcn
   if aggregator_type not in ['mean', 'pool', 'lstm', 'gcn']:
       raise KeyError('Aggregator type {} not supported.'.format(aggregator_type))
   if aggregator_type == 'pool':
       self.fc_pool = nn.Linear(self._in_src_feats, self._in_src_feats)
   if aggregator_type == 'lstm':
       self.lstm = nn.LSTM(self._in_src_feats, self._in_src_feats, batch_first=True)
   if aggregator_type in ['mean', 'pool', 'lstm']:
       self.fc_self = nn.Linear(self._in_dst_feats, out_feats, bias=bias)
   self.fc_neigh = nn.Linear(self._in_src_feats, out_feats, bias=bias)
   self.reset_parameters()
   ```

3. 注册参数和子模块
   - 根据聚合类型，子模块有所不同
   - 调用reset_parameters()进行权重初始化

## 3.2 forward

---

### forward 函数

三个功能

1. 检测输入图对象合规
2. 消息传递和聚合
3. 聚合后更新特征作为输出

### 输入图对象合规检测

```python
def forward(self, graph, feat):
    with graph.local_scope():
        # 指定图类型，然后根据图类型扩展输入特征
        feat_src, feat_dst = expand_as_pair(feat, graph)
```

### 消息传递和聚合

所有消息传递都使用 update_all()实现

### 更新输出

```python
# 激活函数
if self.activation is not None:
    rst = self.activation(rst)
# 归一化
if self.norm is not None:
    rst = self.norm(rst)
return rst
```

根据构造函数中设置的选项来应用激活函数和进行归一化

## 3.3 GraphConv

----

异构图上的 GNN 模块

1. 每个关系上的 DGL NN 模块
2. 聚合来自不同关系上的结果

$$
h_{d s t}^{(l+1)}=\underset{r \in \mathcal{R}, r_{d s F}=d s t}{A G G}\left(f_{r}\left(g_{r}, h_{r_{s r c}}^{l} h_{r_{d s t}}^{l}\right)\right)
$$

其中 𝑓𝑟 是对应每个关系 𝑟 的NN模块，𝐴𝐺𝐺 是聚合函数。

### 实现逻辑

# chapter4  Graph Data Pipeline

如何为用户自己的图数据创建一个DGL数据集

DGL在 [dgl.data](https://docs.dgl.ai/api/python/dgl.data.html#apidata) 里实现了很多常用的图数据集。它们遵循了由 [`dgl.data.DGLDataset`](https://docs.dgl.ai/generated/dgl.data.DGLDataset.html#dgl.data.DGLDataset) 类定义的标准的数据处理管道。 DGL推荐用户将图数据处理为 [`dgl.data.DGLDataset`](https://docs.dgl.ai/generated/dgl.data.DGLDataset.html#dgl.data.DGLDataset) 的子类

