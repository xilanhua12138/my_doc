Mip-Nerf是在近年来被广泛的文章作为baseline的一篇经典。mip-NeRF通过有效地渲染抗锯齿锥形视锥体而不是光线，减少了令人反感的混叠伪影，并显着提高了 NeRF 呈现精细细节的能力。
![[Pasted image 20231005215848.png]]

我们将会在这里较详细的讲解Nerf。

Mip-Nerf的关键是在于其对传统的nerf采样方法进行改进，在原本的nerf中，如果要求解新视角中某个像素的颜色，步骤可以认为是，只沿着一条光线采样若干个点，然后对这些点进行PE，然后代入MLP，然后进行渲染。由于在采样时，只按照一条光线采样，所以当训练集中的图片分辨率较低时，图像中一个像素点的像素值可能来自于原本真实空间中若干个点的集合，而原始Nerf只沿着空间中一条光线采样，这会导致混叠和模糊。

在Mip-Nerf的改进中，把原始只按照一条光线进行采样改进为从一个圆锥中进行采样。这不仅改进了混叠的情况，而且这样符合真实的视觉情况。
![[Pasted image 20231005004156.png]]

什么叫做从一个圆锥中采样呢？

可以理解为，沿着射线发出一个圆锥，原始Nerf中的射线上的采样点转变为了如图所示的圆台，这里圆台里面所有包含的点就是我们需要进行采样的点，因为这些所有的点就是视锥范围内所“看到”的点。

但是，这里遇到一个问题，我们怎么知道包括了多少个点呢？这些点在什么位置呢？所有本文引入了二维高斯分布，我们认为这些点在圆台内是按照二维高斯分布的，即在这些点的位置存在一个均值（我们认为在射线上），然后射线方向和垂直射线方向上分别存在一个方差。
![[Pasted image 20231005213150.png]]
在这之后，我们回到渲染一个像素点的流程，我们采样的目的就是需要知道：我目前要渲染的这个像素点是来自空间中哪些点的信息？

那么我们通过上述的过程，对这个圆台里所有的点的位置求期望，就可以知道我们每个像素点来自空间中哪些点的信息了。
![[Pasted image 20231005213211.png]]
![[Pasted image 20231005213218.png]]

接下来，通过推导求得PE表达式，则后续过程和原始的Nerf一致。

不足之处