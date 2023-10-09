backbone: Mip-Nerf
创新：
新加了两个Loss
保证Nerf预测的深度和DPT预先设置好的深度预测模型一致
1. 深度连续Loss
保证Nerf预测的深度相对值和DPT模型预测的深度相对值一致
![[Pasted image 20230919114421.png]]

2. 空间连续Loss
![[Pasted image 20230919114454.png]]
