![[Pasted image 20240516225407.png]]
![[Pasted image 20240516225415.png]]
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
