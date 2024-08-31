[【踩坑记录】colmap中的相机位姿的坐标系定义及其可视化结果的隐含转换_colmap位姿估计-CSDN博客](https://blog.csdn.net/weixin_44120025/article/details/124604229)

[三维重建基础： 坐标系 (更新中)_colmap 世界坐标-CSDN博客](https://blog.csdn.net/flow_specter/article/details/127805896)

[【三维视觉】相机模型和旋转矩阵 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/405306563)

[身处相机内外参之间（EG3D/NeRF/3D Gaussian Splatting） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/691419487)

实验数据永远是opencv的w2c矩阵，也就是如下
![[a71bcec9c7dd8f9a0ec0d4f6223c8b0.jpg]]


外参矩阵（W2C）
外参矩阵是w2c，姿态是c2w
opencv标定得到的是w2c

[相机位姿(camera pose)与外参矩阵 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/642715876)
c2w矩阵的每一列就是相机的坐标轴对应世界坐标系中的向量方向

[Math Derivation of 3D Gaussian Splatting | 我起初心向明月 (zjwfufu.github.io)](https://zjwfufu.github.io/2023/11/11/3DGS_math/)


# 我的理解
相机外参矩阵是w2c，而c2w是外参矩阵的逆矩阵，一般称为位姿
[相机位姿(camera pose)与外参矩阵 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/642715876)
重点看以上的这篇文章。
很重要的一点是