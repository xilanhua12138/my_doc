![[Pasted image 20240516225407.png]]

![[Pasted image 20240516225437.png]]
其中W的形状为（pixel_num, voxel_num），W中每个元素的计算方法为：
```python
def calculate_weights(voxels, rays, voxel_size):

    weights = np.zeros((len(rays), len(voxels)))
    for i, ray in enumerate(rays):
        ray_origin, ray_direction = ray
        for j, voxel_center in enumerate(voxels):
            intersect_volume = calculate_intersecting_volume
            voxel_volume = np.prod(voxel_size)
            weights[i, j] = intersecting_volume / voxel_volume

    return weights

```
求亚体素是否和光线相交要使用AABB算法：
[GAMES101-现代计算机图形学入门09（光线追踪）_轴对齐包围盒-CSDN博客](https://blog.csdn.net/qq_62214161/article/details/129505762)
[【Games101 作业五/六】Whitted-Style光线追踪(2)：AABB与BVH加速光线求交 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/691928039)
![[Pasted image 20240516232242.png]]
迭代用这个公式就行了 ，源自[1-s2.0-S0030401822003613-main.pdf](file:///D:/Desktop/1-s2.0-S0030401822003613-main.pdf)
![[Pasted image 20240516225415.png]]![[1-s2.0-S0030401822003613-main.pdf]]