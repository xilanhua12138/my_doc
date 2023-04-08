[[LLaMA模型]]采用SwiGLU替换了原有的ReLU。
采用SwiGLU的FNN，在论文中以如下公式进行表述:
$$
F F N_{s w i G L U}\left(x, W, V, W_2\right)=\left(S wish_1(x W) \otimes x V\right) W_2
$$
其中， $S w i \operatorname{sh}_\beta(x)=x \sigma(\beta x)$, (Ramachandran et al., 2017.)
$$\text{silu}(x) = x * \sigma(x), \text{where } \sigma(x) \text{ is the logistic sigmoid.}$$
这里Swish和SiLU是等价