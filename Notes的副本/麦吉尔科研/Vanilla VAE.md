Vanilla VAE

latent_dim：隐变量维度

hidden_dims：隐藏层层数（卷积）





加入高表达基因权重：

1. 在预处理程序里加入语句，表达掩码会添加到 adata.var √
2. 写入文件 √ 
3. 在初始化数据集环节读取掩码作为属性 
4. 写掩码矩阵的 get 方法
5. 写 train_with_highly_variable_mask 函数，比 train 函数多一个参数（掩码矩阵）
6.  在传参时使用 get 方法调用掩码来传递
7. 写 loss_with_weight 方法
8. 在 train_with_highly_variable_mask 中调用