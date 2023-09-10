这篇文章和[[Nerfies]]中的Coarse to fine非常像啊不知道为啥还能单独发一篇顶会。


## Frequency Regularization

Nerfies中是这样：
粗到细退火的位置编码位置γα(x)
![[Pasted image 20230907022348.png]]
![[Pasted image 20230907022624.png]]
这个玩意的取值就只有两个，要么$\omega=1,\alpha-j>0$ ，要么$\omega=0,\alpha-j<0$。换句话说，就是频率大于$\alpha$的被值为零，然后$\alpha$是随着训练的轮次而增加的，也就是随着训练的进行，被掩盖住的PE越来越少。
👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆
**真会讲故事**

FreeNerf中是这样：
Our frequency regularization circumvents the unstable and susceptible highfrequency signals at the beginning of training and gradually provides NeRF high-frequency information to avoid oversmoothness.
我们的频率正则化在训练开始时绕过了不稳定和易受影响的高频信号，并逐渐提供NeRF高频信息以避免过度平滑
![[Pasted image 20230909112009.png]]
![[Pasted image 20230909112017.png]]
We note that our frequency regularization shares some similarities with the coarse-to-fine frequency schedules used in other works [23, 16]. Different from theirs, our work focuses on the few-shot neural rendering problem and reveals the catastrophic failure patterns caused by highfrequency inputs and their implication to this problem.
与他们不同的是，我们的工作专注于少镜头神经渲染问题，并揭示了高频输入引起的灾难性故障模式及其对该问题的影响。
**你们也知道有点像啊**
![[Pasted image 20230909112159.png]]
![[Pasted image 20230909112216.png]]

## Occlusion Regularization

局限性：
**太straight了这个想法，万一前景是我要的东西呢？**
![[Pasted image 20230909113206.png]]
其中mk是一个二进制掩码向量，用于确定一个点是否会受到惩罚，σK表示沿射线采样的K个点的密度值，按接近原点的顺序（从近到远）。为了减少相机附近的固体漂浮物，我们将mk到索引M（称为正则化范围）的值设置为1，其余值设置为0。遮挡正则化损失易于实现和计算。

**意思就是**
这个 occlusion regularization loss 的目的是为了减少视角中靠近相机的浮体(floater)。具体方法是对沿射线采样的点进行惩罚,靠近相机的点惩罚越大,远离相机的点惩罚越小。
通过设定一个 regularization range M, 在这个范围内的点 mk 被置为1,也就是会受到惩罚;超出这个范围的点 mk 被置为0,不受惩罚。这样可以减少相机附近不合理的浮体出现。这个损失函数计算简单有效,是一种简单的遮挡正则化方法。


