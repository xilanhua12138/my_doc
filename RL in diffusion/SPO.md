目前问题：DPO会把prefer lable沿着整条采样路径进行传播，可能导致应用了prefer之后出现图片quality下降的情况
解决方法：通过将采样步骤单独出来。为每一步采样都进行一个win和lose的判断，然后正常采样，能保证$x_{t-1}$是沿着原来模型的mou'ti

要额外训一个step aware preference model
![[Pasted image 20240907154416.png|450]]
step aware model
![[Pasted image 20240907154636.png]]