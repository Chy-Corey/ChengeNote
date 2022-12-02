## The Minimum Error Minimax Probability Machine

$Hongyu\ Chen$

$Huazhong\ University\ of\ Science\ and\ Technology$

---

### Abstract

> distribution-free Bayes optimal classifier called the Minimum Error Minimax Probability Machine (MEMPM) in a worst-case setting

如上所属，这是一个非参数的贝叶斯优化分类器，我们先介绍一下贝叶斯分类器:

经典的朴素贝叶斯分类器，有一个很大缺陷：假设所有的特征分量是独立的，但是在多数情况下，各个特征之间会有或多或少的关系。这篇文章的算法有两个重要特征：非参数、最坏情况优化。

**非参数**

已知样本所属的类别，但未知总体概率密度函数的形式，要求我们直接推断概率密度函数本身。统计学中常见的一些典型分布形式不总是能够拟合实际中的分布。此外，在许多实际问题中经常遇到多峰分布的情况，这就迫使我们必须用样本来推断总体分布，常见的总体类条件概率密度估计方法有Parzen窗法和Kn近邻法两种。
非参数估计也有人将其称之为无参密度估计，它是一种对先验知识要求最少，完全依靠训练数据进行估计。

这篇文章不需要对数据进行分布约束，可以通过取样估计样本的均值、方差等，但是也会带来一些缺点，比如如果无法精确估计样本特征，训练后的结果会存在误差。

**最坏情况优化**

大多数分类器，仅对数据本身进行了训练，这样可能会导致训练准确率很高，但是测试的准确率却不能bound住，本篇文献对最坏情况进行了最优化，这样测试准确率就一定会高于训练准确率，从而可以很好地bound。



### Minimum Error Minimax Probability Decision Hyperplane

> This derived decision hyperplane is claimed to minimize the worst-case (maximal) probability of misclassification, or the error rate, of future data.

这里诠释了**最坏情况优化**：

$\begin{array}{ll}
\max _{\alpha, \mathbf{a} \neq \mathbf{0}, b} & \alpha \quad \text { s.t. } \\
& \inf _{\substack{\mathbf{x} \sim\left(\overline{\mathbf{x}}, \Sigma_{\mathbf{x}}\right)}} \operatorname{Pr}\left\{\mathbf{a}^{T} \mathbf{x} \geq b\right\} \geq \alpha \\
& \inf _{\substack{\mathbf{y} \sim\left(\overline{\mathbf{y}}, \Sigma_{\mathbf{y}}\right)}} \operatorname{Pr}\left\{\mathbf{a}^{T} \mathbf{y} \leq b\right\} \geq \beta
\end{array}$

在x和y所属的分布中，要使分类正确率的下线大于限定值。也就是在最坏的分类情况时，也能保证正确率大于限定值。

关于如何找到分布的下限，论文直接给出了结论(lemma3)，并指明了进行结论推导的文献，可自行查阅。



### Robust Version

鲁棒性版本，其实是在未知样本的参数(均值向量、协方差矩阵)时，对应的解决方法。

$\begin{array}{l}
\max _{\alpha, \beta, \mathbf{a} \neq \mathbf{0}, b} \quad\theta \alpha+(1-\theta) \beta \quad \text { s.t. } \\
\inf _{\mathbf{x} \sim\left(\overline{\mathbf{x}}, \Sigma_{\mathbf{x}}\right)} \quad\operatorname{Pr}\left\{\mathbf{a}^{T} \mathbf{x} \geq b\right\} \geq \alpha, \forall\left(\overline{\mathbf{x}}, \Sigma_{\mathbf{x}}\right) \in \mathcal{X} \\
\inf _{\mathbf{y} \sim\left(\overline{\mathbf{y}}, \Sigma_{\mathbf{y}}\right)} \quad\operatorname{Pr}\left\{\mathbf{a}^{T} \mathbf{y} \leq b\right\} \geq \beta, \forall\left(\overline{\mathbf{y}}, \Sigma_{\mathbf{y}}\right) \in \mathcal{Y}
\end{array}$

这里首先对上面的均值和协方差的分布进行了限制，然后写出了其分布：

$\begin{array}{r}
X=\left\{\left(\overline{\mathbf{x}}, \Sigma_{\mathbf{x}}\right) \mid\left(\overline{\mathbf{x}}-\overline{\mathbf{x}}^{0}\right) \Sigma_{\mathbf{x}}^{-1}\left(\overline{\mathbf{x}}-\overline{\mathbf{x}}^{0}\right) \leq v_{\mathbf{x}}^{2},\left\|\Sigma_{\mathbf{x}}-\Sigma_{\mathbf{x}}^{0}\right\|_{F} \leq \rho_{\mathbf{x}}\right\} \\
Y=\left\{\left(\overline{\mathbf{y}}, \Sigma_{\mathbf{y}}\right) \mid\left(\overline{\mathbf{y}}-\overline{\mathbf{y}}^{0}\right) \Sigma_{\mathbf{y}}^{-1}\left(\overline{\mathbf{y}}-\overline{\mathbf{y}}^{0}\right) \leq v_{\mathbf{y}}^{2},\left\|\Sigma_{\mathbf{y}}-\Sigma_{\mathbf{y}}^{0}\right\|_{F} \leq \rho_{\mathbf{y}}\right\}
\end{array}$

第一个限制，类似于正太分布的公式，但是我不知道为什么要这么限制。第二个限制，对协方差矩阵的trace进行了限制，以我的理解来看，协方差矩阵可以化为正对角矩阵，它的trace就是对角线的和，即限制了样本中向量的分布，样本总体的分散情况由这个条件限制。



### How Tight Is the Bound

这一段解释了训练正确率和测试正确率的关系(Bound程度)。作者奇怪的发现，虽然偏置(bias)训练正确率($\beta$)已经下降到0，但是测试时，还能达到50%正确率，作者对此进行了解释。

大体意思为：即便数据的均值向量和协方差矩阵不变，数据本身也会存在一定的偏移，当把数据向左偏时，bound会变松，把数据向右移动时，bound就会变紧。我觉得作者太牛了。















