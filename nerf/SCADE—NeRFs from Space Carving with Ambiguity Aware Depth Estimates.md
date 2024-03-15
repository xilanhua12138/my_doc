[2303.13582.pdf (arxiv.org)](https://arxiv.org/pdf/2303.13582.pdf)
# 核心思路
#深度监督

**对齐两个分布**
分布1：深度估计器的深度distribution
分布2：Nerf的深度估计

对齐方法：
因为深度存在ambiguity，但是深度估计器(LeReS)只能估计一个point wise的深度，所以在深度估计器中引入randomness从而对于一张图估计**好几个**深度，这些**好几个**可以认为是从这个图所对应的深度分布采样出来的sample，所以后续通过对齐这些样本就可以实现对于深度分布的对齐。这个方法是搬用了StyleGAN的思路进行多个深度的生成。
![[Pasted image 20231205163258.png]]


其次对于Nerf，其光线终止点可以认为是Nerf对深度的估计，而光线终止点的概率密度函数可以由此表达出来：
$$f_{\theta, \mathbf{o}, \mathbf{d}}(t)=T_{\theta, \mathbf{o}, \mathbf{d}}(t) \sigma_\theta(\mathbf{o}+t \mathbf{d})$$
从而通过对其概率累积函数采样（CDF），可以得到在不同概率下的深度：
$$
\begin{equation}
\begin{array}{r}
x_i=F_{\theta, \mathbf{o}, \mathbf{d}}^{-1}\left(u_i\right), \\
\text { where } u_i \sim \mathcal{U}(0,1)
\end{array}
\end{equation}
$$
可以参考与GPT4的对话来理解：
![[Pasted image 20231205163141.png]]
![[Pasted image 20231205163159.png]]
最后基于上述两个采样出来的深度sample，就可以设计一个损失函数：
![[Pasted image 20231205163420.png]]
这个损失函数是依照conditional Implicit Maximum Likelihood Estimation (cIMLE）设计出来的，min是取一个距离最近的，然后求二范数，总的来说可以理解为，对于每一个x_i(Nerf的深度)，选一个离他最近的y_j然后求MSE，然后求和所有的i

最后总的loss为：
![[Pasted image 20231205163830.png]]
