![[Pasted image 20230828113439.png]]
可以认为Nerfies = D-Nerf + Nerf-W + Tricks

#### 值得学习的地方：
1. 粗到细退火的位置编码位置γα(x)
![[Pasted image 20230907022348.png]]
![[Pasted image 20230907022624.png]]
这个玩意的取值就只有两个，要么$\omega=1,\alpha-j>0$ ，要么$\omega=0,\alpha-j<0$。换句话说，就是频率大于$\alpha$的被值为零，然后$\alpha$是随着训练的轮次而增加的，也就是随着训练的进行，被掩盖住的PE越来越少。
👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆👆
**真会讲故事**

2. Elastic Regularization：
   雅各比矩阵：
   ![[Pasted image 20230828114257.png]]
   例如
   $F = (y_1,y_2,y_3,y_4)=f(x_1,x_2,x_3,x_4)$![[Pasted image 20230828114333.png]]
   ![[Pasted image 20230828115117.png]]
   
   
