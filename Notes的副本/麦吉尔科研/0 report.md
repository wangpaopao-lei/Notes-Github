## Contents

[toc]



## Q1 Basics of VAE

1. **forward step**: The forward step of a Variational AutoEncoder (VAE) involves two parts: firstly, an encoder network maps the input data into a latent space by learning a mean and a standard deviation, defining a Gaussian probability distribution within the latent space. Secondly, a decoder network uses a sample from this distribution to reconstruct the input data.

2. **generate new samples**: To generate new samples in a VAE, a point is randomly selected from the latent space based on the learned Gaussian distribution. This point is then input into the decoder network, which generates a new sample that should theoretically resemble the types of data the VAE was trained on.

3. To generate new samples that are similar to a specific input sample, you firstly encode this specific input sample to its latent space representation. This provides the mean and standard deviation parameters of a Gaussian distribution that is thought to encapsulate the 'style' or 'characteristics' of that specific input. Then, you sample a point from this specific Gaussian distribution and pass it through the decoder. The resulting output will be a new sample sharing similarities with the specific input sample.

## Q2 Dataset

Cell: 3005

Gene: 19972

Type(label): 7

Type(label2): 48



<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/Untitled.png" alt="Untitled" style="zoom:50%;" />

## Q3 Preprocess

I took 3 steps of preprogress:

1. total-count normalize
2. logarithmize
3. scale gene to unit variance and clip

```Python
sc.pp.normalize_total(anndata_object, target_sum=1e4)
sc.pp.log1p(anndata_object)
sc.pp.scale(anndata_object,max_value=10)
```

## Q4 Implement

Given the characteristics of the single-cell dataset, I used a fully connected network with three layers to construct encoder and decoder, where the number of nodes decreases layer by layer. ReLU is utilized as the activation function.

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/%E6%9C%AA%E5%91%BD%E5%90%8D.png" alt="未命名" style="zoom:50%;" />

```python
class VAE(nn.Module):
    def __init__(self, input_dim, latent_dim, drop_prob=0.3) -> None:
        super(VAE, self).__init__()
        self.latent_dim = latent_dim
        h_dim = [1024, 512, 256]

        layers = []
        for i in range(len(h_dim)):
            layers.append(nn.Linear(input_dim if i == 0 else h_dim[i - 1], h_dim[i]))
            layers.append(nn.ReLU())
        layers.append(nn.Linear(h_dim[-1], latent_dim * 2))
        self.encoder = nn.Sequential(*layers)

        layers = []
        for i in range(len(h_dim) - 1, -1, -1):
            layers.append(nn.Linear(latent_dim if i == len(h_dim) - 1 else h_dim[i + 1], h_dim[i]))
            layers.append(nn.ReLU())
        layers.append(nn.Linear(h_dim[0], input_dim))
        self.decoder = nn.Sequential(*layers)

    def reparameterize(self, mu, log_var):
        std = torch.exp(0.5 * log_var)
        eps = torch.randn_like(std)
        z = mu + eps * std
        return z

    def forward(self, X):
        out = self.encoder(X)
        mu = out[:, :self.latent_dim]
        log_var = out[:, self.latent_dim:]
        z = self.reparameterize(mu, log_var)
        X_hat = self.decoder(z)
        return X_hat, z, mu, log_var

    def recon_loss(self, X, X_hat):
        return F.mse_loss(X, X_hat)

    def kl_loss(self, mu, log_var):
        return torch.mean((-0.5 * (1 + log_var - torch.square(mu) - log_var.exp()).sum(dim=1)), dim=0)
```

### Umap result

The result of extracting latent variables and performing UMAP dimensionality reduction is shown below:

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230709173114344.png" alt="image-20230709173114344" style="zoom:50%;" />





## Q5 Evaluate

The final ARI score is **0.859**

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230709173157834.png" alt="image-20230709173157834" style="zoom:50%;" />

For comparison, the PCA trained with the same dataset and the same number of main components as the latent variable dimensions scored **0.587**

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230709174923219.png" alt="image-20230709174923219" style="zoom:50%;" />

## Q6 Highly variable genes

Following the integration of high variability gene weights, the Adjusted Rand Index (ARI) score did not exhibit the expected increase, but rather underwent a significant decrease.

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/image-20230709205317087.png" alt="image-20230709205317087" style="zoom:50%;" />

After several attempts at adjusting the weights, the best score achieved hovered around **0.65**. This result may be attributable to several factors:

1. **Inappropriate Weight Settings**: During the experiments, weights ranging between 1.5 and 20 were attempted, but the results demonstrated minimal differences and failed to exhibit a discernible pattern. It is plausible that the optimal weight does not reside within this tested range.
2. **Model Structural Adjustments**: The introduction of weights has increased the complexity of the model. Accordingly, adjustments to the model's structure or the training process might be necessary to attain optimal performance.
3. **High Variability Genes May Not Be the Optimal Features**: While high variability genes theoretically contain more information, it does not automatically denote their utmost importance for this specific task.

## *Q7 Improve

### β-VAE--the most important

Initially, the loss was obtained directly by adding the reconstruction loss and KL divergence. However, I encountered a problem of KL Divergence Collapse. Plus, the scale gap between KL divergence and reconstruction loss was significant, leading the model to ignore the reconstruction loss and only focus on KL divergence. This means that the contribution of the latent variables to data generation was minimal, with most information being modeled through the decoder rather than the latent variables. As a result, the latent variables trend towards the prior distribution, but they are irrelevant in reality, so the ARI score approaches 0 (random distribution).

After identifying this, I added a weight to the KL divergence when calculating the loss, changing the loss calculation to:
$$
Loss=recon\_loss+\beta*KLD
$$
o validate this idea, I set β to 0, resulting in an ARI of **0.462**. The model's output of latent variables contained some information, but the performance was mediocre, even less than that of PCA.

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/klw0.png" alt="klw0" style="zoom:50%;" />

To explore the evolution of KL divergence, I set β to a very small value, here $10^{-7}$, and retrained the model. The ARI then achieved a substantial increase to **0.718**, with the loss evolution during training shown below:

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/klwlow%20curve.png" alt="klwlow curve" style="zoom:50%;" />

This result contains the following information:

1. Under this weight, the model can consider the role of KL divergence as a loss.
2. The KL divergence starts from 0 and increases with training, then shows a weak downward trend.

From here, I applied the **warm-up** strategy to the weight of the KL divergence. As a baseline, I used the epoch as the weight of the weight. Now the β calculation is:
$$
\beta=10^{-7}*epoch
$$
This means that at the beginning of the training, a smaller weight is applied to the KL divergence so that the model focuses more on the reconstruction loss. In the later stage, the model starts to pay attention to the KL divergence

<img src="https://wangleidetuchuang.oss-cn-beijing.aliyuncs.com/img/v2.png" alt="v2" style="zoom:50%;" />



During this training process, I was finally able to achieve a decrease in all three losses. Simultaneously, by analyzing the changes in the Kullback-Leibler (KL) divergence, I identified a crucial insight: around the 20th epoch, the KL divergence began to demonstrate an optimal downward trend.

Based on this observation, I revised my warm-up strategy. I recalculated β according to the formula:
$$
\beta=10^{-7}*\theta
$$
where θ starts at 1 and increases with each epoch, peaking at epoch 20. By implementing this strategy, the ARI score improved to **0.859**, which was my final result.



### Potential Strategies for Further Improvement

1. **Hyperparameter tuning**: Limited by computational resources, I did not conduct extensive hyperparameter tuning. If resources permit, techniques like grid search can be used for hyperparameter tuning, potentially leading to better performance.
2. **Network architecture**: Given the size of the dataset, I employed a six-layer fully connected network architecture, without careful consideration of the number of nodes. Implementing a more suitable network structure may lead to better results.

