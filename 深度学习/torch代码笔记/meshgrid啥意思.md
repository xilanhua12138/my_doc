```python
x, y = torch.meshgrid(torch.arrange(5), torch.arrange(4))
```
分别对变量x和y都会返回一个大小为5×4的网格，5行4列，我想知道这个5行4列任意一个位置的坐标的x坐标或者y坐标就对其进行索引：
例如，我想知道（3，2）对应的x坐标，就用$x[3][2]$就可以了，因为这里是顺着变化的，看起来没意义，我知道是3，2坐标是3为啥还要再索引一遍呢？

所以一般用于不是连续变化的，例如x的范围是$[1,2,4,6,7]$ y的范围是$[2,5,7]$
或者坐标是归一化坐标的时候

```python
    uv = torch.stack(torch.meshgrid(torch.arange(resolution),torch.arange(resolution), indexing='ij')) * (1./resolution) + (0.5/resolution)
```

除以了一个数然后进行了归一化，这个加0.5是把格点的原点从左上角移动到了中心点

我这里的用法是：
```python
x_min = y_min = z_min = - cube_mean + mean_cam_pos

x_max =  y_max = z_max = (max_cam_pos - min_cam_pos) - cube_mean + mean_cam_pos

resolution = args_dict['grid_resolution']

x = np.linspace(x_min, x_max, resolution)  # 可以根据需要调整点的数量

y = np.linspace(y_min, y_max, resolution)

z = np.linspace(z_min, z_max, resolution)

x,y,z = np.meshgrid(x, y, z, indexing='ij')
```