[极大似然估计、最大后验估计 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/93416473/)

### 贝叶斯公式：
$$
p(\theta \mid X)=\frac{p(X \mid \theta) p(\theta)}{p(X)}
$$

### 极大似然估计
极大似然估计的核心思想是：**认为当前发生的事件是概率最大的事件。因此就可以给定的数据集，使得该数据集发生的概率最大来求得模型中的参数**。似然函数如下：
$$
p(X \mid \theta)=\prod_{x 1}^{x n} p(x i \mid \theta)
$$
为了便于计算，我们对似然函数两边取对数，生成新的对数似然函数（因为对数函数是单调增函数，因此求似然函数最大化就可以转换成对数似然函数最大化）：$$
p(X \mid \theta)=\prod_{x 1}^{x n} p(x i \mid \theta)=\sum_{x 1}^{x n} \operatorname{log}p(x i \mid \theta)
$$
求对数似然函数最大化，可以通过一阶优化算法如sgd或者二阶优化算法如Newton求解。
$$
argmax\sum_{x 1}^{x n} \operatorname{log}p(x i \mid \theta)
$$

### 最大后验估计
在最大后验概率估计中，我们要求后验概率最大的参数值 $\theta_{MAP}$，其中 $X$ 是观测到的数据。

根据贝叶斯定理，后验概率可以表示为：
$$
p(\theta \mid X)=\frac{p(X \mid \theta) p(\theta)}{p(X)}
$$
其中，$p(X | \theta)$ 是给定参数 $\theta$ 时数据 $X$ 出现的概率，$p(\theta)$ 是参数 $\theta$ 的先验分布，$p(X)$ 是数据 $X$ 出现的概率。这里的分母 $p(X)$ 是一个归一化因子，确保概率分布的总和为1。

在求解最大后验概率时，我们只需要找到使后验概率最大的参数 $\theta_{MAP}$，因此可以忽略分母 $p(X)$。因为分母 $p(X)$ 是一个与参数 $\theta$ 无关的常数，而我们的目标是找到最大化后验概率的参数 $\theta_{MAP}$，因此可以省略该常数。

具体来说，我们可以只考虑分子 $p(X | \theta) p(\theta)$ 的最大值，而不用考虑分母 $p(X)$ 的值。因此，我们可以将求解最大后验概率问题转化为以下优化问题：
$$
\theta_{M A P}=\arg \max _\theta p(\theta \mid X)=\arg \max _\theta p(X \mid \theta) p(\theta)
$$
这个优化问题与最大似然估计问题非常相似，只是在似然函数中引入了先验分布的信息。
$$
\operatorname{argmax}\left(\sum_{x 1}^{x_n} \operatorname{logp}(x_i \mid \theta)+\operatorname{logp}(\theta)\right)
$$
## 由这里还可以推出[[L1和L2分别对应哪种先验分布？为什么？]]