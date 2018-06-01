---
title: Kalman Filter II(Chinese)
tags: 
- Maths
categories: Maths
---
# （二）Kalman Filter公式推导
考虑一个用状态空间模型描述的动态系统
$$X(t+1)=\Phi X(t) + \Gamma W(t) $$ $$Y(t)=H X(t) + V(t)$$其中，$t$ 为时间，系统在时刻 $t$ 的状态为 $X(t) \in R^n$ ；Y(t)为观测信号； $W(t) \in R^r$ 为输入的噪声， $V(t)\in R^m$ 为观测噪声。由于不同问题的状态空间描述是不一样的，因此只有在确定状态空间后才可以得到递推公式（这是我自己的看法，不知道对不对，可能递推公式还是一样，不过根据问题来推一遍公式总不会有错的，只是麻烦点）。

###	【假设1】
-  $W(t)$ 和 $V(t)$ 是均值为零，方差各为 $Q$ 和 $R$ 的不相关白噪声
###	【假设2】
- 初始状态 $X(0)$ 与 $W(t)$ 和 $V(t)$ 不相关，而且
$$E[X(0)] = \mu_0,E[(X(0)-\mu_0)(X(0)-\mu_0)^T] = P_0$$

##	推导
- **问题重述**

卡尔曼滤波是一个基于观测信号 $\{Y(1),Y(2),...,Y(t) \}$ ，求状态 $X(k)$ 的线性最小方差估计值的问题 $\hat X(k|t)$ ，而且这个问题的目标函数是
$$J=E[(X(k)-\hat X(k|t))^T(X(k)-\hat X(k|t))]$$那么由之前的定义1.1可以得到，卡尔曼滤波问题可以归为求射影
$$\hat X(k|t) = proj(X(k)|Y(1),Y(2),...,Y(t))$$由定义1.5可以得到递推公式为
$$\hat X(t+1|t+1) = \hat X(t+1|t)+K(t+1)\epsilon (t+1)$$$$
K(t+1)=E[X(t+1)\epsilon^T(t+1)]\{ E[\epsilon (t+1)\epsilon^T(t+1)]\}^{-1}$$其中$K(t+1)$ 为卡尔曼滤波器增益。

- **对状态方程求射影**
 
以 $w$ 表示观测信号，对状态方程 $X(t+1)=\Phi X(t) + \Gamma W(t) $ 两边取射影有（射影运算具有分配律，跟向量点乘一样）
$$proj(X(t+1)|w) = \Phi proj( X(t)|w)+\Gamma proj(W(t)|w)$$$$\hat X(t+1|t) = \Phi \hat X(t|t)+\Gamma proj(W(t)|w)$$迭代求解状态方程后获得原状态方程的另一种表示
$$X(t+1) = \Phi^{t+1}X(0) + \Gamma [\Phi^0 W(t)+\Phi^1 W(t-1)+...+\Phi^t W(0)]$$对于没有控制量的状态方程，由于假设1的白噪声的性质和假设2，我们可以知道 $X(0),W(0),...,W(t)$ 互不相关，即正交，因此可以把它们看作基，而 $X(t+1)$ 是可以由它们表示，所以有
$$X(t+1) \in L(W(t),...,W(0),X(0))$$当然，如果状态方程里面有控制量$U(t)$的加入，即
$$X(t+1)=\Phi X(t) + \Psi U(t) + \Gamma W(t) $$相应递推公式为
$$X(t+1) = \Phi^{t+1}X(0) +[\Phi^0 \Psi U(t)+\Phi^1 \Psi U(t-1) +...+\Phi^t\Psi U(0)]+ [\Phi^0 \Gamma W(t)+\Phi^1\Gamma W(t-1)+...+\Phi^t\Gamma W(0)] $$而有控制量的状态方程，我们认为每个时刻的控制量都是与公式中其它变量不相关，所以同理有
$$X(t+1) \in L(U(t),...,U(0),W(t),...,W(0),X(0))$$

- **对观测方程求射影**

考虑上文中不带控制量的状态方程的推导，同理，对观测方程做同样的运算有
$$Y(t+1) \in L(V(t+1),W(t)...,W(0),X(0))$$所以有
$$L(Y(1),Y(2),...,Y(t)) \subset L(V(t),...,V(1),W(t-1),...,W(0),X(0))$$

- **状态方程和观测方程的递推式**

由于假设，可以知道 $W(t)$ 与 $ \{V(t),...,V(1),W(t-1),...,W(0),X(0)\}$不相关（正交），所以有
$$W(t) \bot L(Y(1),Y(2),...,Y(t))$$因为正交，所以投影后的状态方程尾项为零，即
$$\hat X(t+1|t) = \Phi \hat X(t|t)$$对观测方程做同样的推导可以同理得到
$$V(t+1) \bot L(Y(1),Y(2),...,Y(t))$$$$\hat Y(t+1|t) =H \hat X(t+1|t) $$

- **协方差更新公式推导**

将滤波器的预报估计值误差和协方差记为
$$\widetilde X(t|t) = X(t)-\hat X(t|t)$$ $$\widetilde X(t+1|t)=X(t+1)-\hat X(t+1|t)$$ $$P(t|t) = E[\widetilde X(t|t) \widetilde X^T(t|t)]$$ $$P(t+1|t) = E[\widetilde X(t+1|t)] \widetilde X^T(t+1|t)]$$由之前的定义1.3，引入新息（不需要管这个名词怎么来的）的表达式
$$\epsilon(t+1) = Y(t+1)-proj(Y(t+1)|Y(1),Y(2),...,Y(t))=Y(t+1)-\hat Y(t+1|t)=[H X(t+1)+V(t+1)]-[H\hat X(t+1|t)] = H \widetilde X(t+1|t)+V(t+1)$$下面进行估计值的误差和协方差的推导
$$\widetilde X(t+1|t) = X(t+1)-\hat X(t+1|t)$$$$
= [\Phi X(t)+\Gamma W(t)]-[\Phi \hat X(t|t)]$$$$
= \Phi \widetilde X(t|t) +\Gamma W(t)$$ 另外一个公式 $$
\widetilde X(t+1|t+1) = X(t+1)-\hat X(t+1|t+1) 
$$$$= X(t+1) - [\hat X(t+1|t)+K(t+1)\epsilon (t+1)] $$$$
= \widetilde X(t+1|t) - K(t+1)\epsilon(t+1) $$$$
= \widetilde X(t+1|t) - K(t+1)[H\widetilde X(t+1|t)+V(t+1)]$$$$
= [I-K(t+1)H]\widetilde X(t+1|t) - K(t+1)V(t+1)$$在计算协方差更新公式时就要利用之前进行射影运算后得到的正交关系。由于$X(t) \in L(W(t-1),...,W(0),X(0))$，而且 $\hat X(t|t)$ 是 $X(t)$ 在流型上的投影（简单把流型理解成空间即可），因此由 $\hat X(t|t)$ 和 $X(t)$ 运算得到的 $\widetilde X(t|t)$ 也属于这个流型，而 $W(t)$ 与这个流型正交，所以有 $W(t) \bot \widetilde X(t|t)$ ，那么在计算协方差更新公式时有

$$P(t+1|t) = E[\widetilde X(t+1|t)] \widetilde X^T(t+1|t)]$$ $$
= E\{ [\Phi \widetilde X(t|t) +\Gamma W(t)][\Phi \widetilde X(t|t) +\Gamma W(t)]^T\}$$$$
= E[\Phi \widetilde X(t|t) \widetilde X^T(t|t) \Phi] +E[\Phi \widetilde X(t|t) W^T(t) \Gamma^T ]+E[\Gamma W(t) \widetilde X^T(t|t)\Phi ]+E[\Gamma W(t)W^T(t)\Gamma^T]$$$$
= \Phi E[\widetilde X(t|t) \widetilde X^T(t|t)]\Phi^T+0+0+\Gamma E[W(t)W^T(t)]\Gamma^T$$ $$
= \Phi P(t|t)\Phi^T+\Gamma Q \Gamma^T$$ （真长(-｡-;)）同理对另一个协方差更新公式进行计算会得到
$$P(t+1|t+1) =[I-K(t+1)H]P(t+1|t)[I-K(t+1)H]^T+K(t+1)R K^T(t+1)$$当然这个公式还不是最简的形式，还可以进一步化简，之前文中已经提到了K(t+1)的表达式，则
$$K(t+1)=E[X(t+1)\epsilon^T(t+1)]\{ E[\epsilon (t+1)\epsilon^T(t+1)]\}^{-1}$$ $$
= E\{[\hat X(t+1|t)+\widetilde X(t+1|t)][H\widetilde X(t+1|t)+V(t+1)]^T\} \{ E[\epsilon (t+1)\epsilon^T(t+1)]\}^{-1}$$ $$
= \{E[\hat X(t+1|t) \widetilde X^T(t+1|t)H^T]+E[\widetilde X(t+1|t)\widetilde X^T(t+1|t)H^T]+E[\hat X(t+1|t)V^T(t+1)]+E[ \widetilde X(t+1|t) V^T(t+1)]\} \{ E[\epsilon (t+1)\epsilon^T(t+1)]\}^{-1}$$ $$
=( 0+P(t+1|t)H^T+0+0)\{ E[\epsilon (t+1)\epsilon^T(t+1)]\}^{-1}$$ $$
=P(t+1|t)H^T E\{  [H \widetilde X(t+1|t)+V(t+1)][ H \widetilde X(t+1|t)+V(t+1)]^T\}^{-1}$$ $$
=P(t+1|t)H^T \{ HP(t+1|t)H^T+R\}^{-1}$$将这个表达式代入上面的协方差更新公式 (基于简化公式的考虑，下面的推导省略时标)
$$P(t+1|t+1) = [I-KH]P[I-KH]^T+KRK^T$$ $$
=[I-KH]P-PH^TK^T+K(HPH^T+R)K^T$$ $$
=[I-KH]P-PH^TK^T+PH^TK^T$$ $$
=[I-KH]P$$即
$$P(t+1|t+1) = [I-K(t+1)H]P(t+1|t)$$至此所有推导已经完成了
