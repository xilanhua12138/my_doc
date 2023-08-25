Insight：利用CNN计算特征引入三维物体的先验知识，对CNN得到的特征进行采样和插值，引入到Nerf中进行重建
![[Pasted image 20230702003641.png]]
Implementation detail：
对于CNN，使用Resnet34，进行下采样
1. 64 × H/2 × W/2 
2. 64 × H/4 × W/4
3. 128 × H/8 × W/8
4. 256 × H/16 × W/16
得到这些特征图之后，将上面的这些特征图，全部上采样至大小为C × H/2 × W/2 ，然后将他们全部concat，得到512 × H/2 × W/2 ，其中512 = 64 + 64 + 128 + 256，得到这个特征之后，在测试阶段，如果要得到某一新视角下的完整的一张图，我们需要知道的是这个新视角对应的相机位姿，然后根据该相机位姿得到所有的光线rays，那我们如何得到新视角的某条光线对应的CNN特征呢？
原文：
Given a input image I of a scene, we first extract a feature volume W = E(I). Then, for a point on a camera ray x, we retrieve the corresponding image feature by projecting x onto the image plane to the image coordinates π(x) using known intrinsics, then bilinearly interpolating between the pixelwise features to extract the feature vector W(π(x)).
我的理解是，将该光纤向量先转化到相机坐标系，然后再转化到像素坐标系，那么这个像素坐标系上的点对应位置直接取CNN特征即可。