[【论文精炼】 | NeRF in the Wild: Neural Radiance Fields for Unconstrained Photo Collections | NeRF in the Wild: 无约束照片集的神经辐射场 | 2021年 - 何雨龙 - 博客园 (cnblogs.com)](https://www.cnblogs.com/noluye/p/14718570.html)
#### GLO
首先，对于N张数据集里的图像x，我们用正态分布随机初始化N个d维的latent code，记为z之，后将z和图像x进行配对，没错，就是这样随机配对
为了与GAN做对比，以证明本文没有用到GAN的鉴别器与生成对抗训练策略便能取得与GAN相似的效果，作者选择了DCGAN的生成器作为本方法的decoder，也是本方法唯一的网络架构
将z丢入decoder，会得到一张图像，这张图像和之前与z配对的图像作loss，反向传播，优化z和decoder

##### 模型管线
将整体场景分为静态物体和瞬态物体，静态物体为真实的目的景观，而瞬态物体可能是任何遮挡物
![[Pasted image 20230828111122.png]]

整体渲染过程：
![[Pasted image 20230828111157.png]]
两个一起渲染

##### 不确定度计算
![[Pasted image 20230828112030.png]]
- 观测的像素强度包含内在噪音:表示我们观测到的像素值并不是真实像素的精确值,而是受到一定随机噪音的影响。
- 偶然的(aleatoric):表示这种噪音是随机的、偶发的,不是我们可以通过改进 sensors 或算法而消除的。
- 异方差的(heteroscedastic):表示这种随机噪音的方差不是固定的,而是依赖于像素的位置或强度等因素。
- 依赖图像和射线的各向同性正态分布:表示我们用一个正态分布来描述像素的噪音。该正态分布的均值是图像在该像素位置的真值C^i(r),方差是βi(r)2,其中r表示射线方向。βi(r)表示噪音的标准差,是依赖于图像强度的函数。各向同性表示噪音在不同方向上是相同分布。

# 局限性与不足
这种方法不能指定天气状况和光线，只能使用插值的方法，并且无法很好的合并两个nerf的latent code  [[Block-Nerf]] 提出了一种方法，通过优化latent code使得两个nerf进行合并

