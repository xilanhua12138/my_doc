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


