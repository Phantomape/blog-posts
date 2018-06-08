---
title: Kalman Filter I(Chinese)
tags: 
- Maths
categories: Maths
---

最近利用卡尔曼滤波来做tracking的东西，然后随手推导了一下。网上大都是只有例子源代码和约定俗成的公式，不太方便使用，毕竟不能保证你遇到的问题恰好能用现有模型来套。所以对滤波器的来由需要深刻地理解一下。

# （一）Kalman Filter原理

##	定义1.1

由 $m\times 1$ 维随机向量 $y\in R^m$ 的线性函数估计 $n\times 1$ 维随机变量 $x\in R^n$ ，估计值记为$$	\hat x = b + Ay ,b\in R^n,A\in R^{n\times m}$$若估计值极小化的指标函数为$$	J = E[(x- \hat x)^T(x- \hat x)]$$则称 $\hat x$ 为随机变量的线性最小方差估计。由观测值 $y$ 求随机变量 $x$ 的线性最小方差估计的表达式为
$$\hat x = E(x)+Cov(x,y)Var(y)^{-1}(y-E(y))$$虽然这个公式好像好难懂，不过暂时不需要完全理解它，后面在推导时可以慢慢领会。同时这个 $\hat x$ 有三个性质

（1）无偏性，$E(\hat x) = E(x)$

（2）正交性，$E[(x-\hat x)y^T] = 0$

（3）不相关性，$x-\hat x$ 与 $y$ 不相关

##	定义1.2
 $x-\hat x$ 与 $y$ 不相关，那么等价于 $x-\hat x$ 与 $y$ 正交（或者理解为垂直），记为 $x-\hat x\bot y$ ，并且称$ \hat x $为 $x$ 在 $y$ 上 的射影，记为$\hat x = proj（x | y） $

##	定义1.3
基于随机变量 $y(1),y(2),...,y(k)\in R^m$，对随机变量 $x\in R^m$ 的线性最小方差估计 $ \hat x$ 定义为$$\hat x = proj(x|w)=proj(x|y(1),y(2),...,y(k))$$也称 $\hat x$ 为 $x$ 在线性流型 $L(w)$ 或 $L(y(1),y(2),...,y(k))$ 上的射影。流型的概念不需要理解，因为后面推导不涉及对它的完全理解
##	定义1.4
设 $y(1),y(2),...,y(k)\in R^m$ 是存在二阶矩的随机序列，它的新息序列（不用纠结名字，其实后面推导里它就是误差序列）定义为
$$\epsilon(k)=y(k)-proj(y(k)|y(1),y(2),...,y(k-1)),k =1,2,...$$并定义的一步最优预报估计值为
$$\hat y(k|k-1)=proj(y(k)|y(1),y(2),...,y(k-1))$$因此新息序列可重新写成
$$\epsilon(k)=y(k)-\hat y(k|k-1),k =1,2,...$$需要规定 $\hat y(1|0)=E[y(1)]$ ，这保证了$E[\epsilon(1)]=0$，所以有$\epsilon(k) \bot L(y(1),y(2),...,y(k-1))$
##	定义1.5
设随机变量 $x\in R^n$ ，随机序列 $y(k)\in R^m$ ，而且随机序列存在二阶矩，则有递推公式（证明以后再补好了(-｡-;)）
$$proj(x|y(1),y(2),...,y(k))=proj(x|y(1),y(2),...,y(k-1))+E[x\epsilon^T(k)]{E[\epsilon(k)\epsilon^T(k)]}^{-1}\epsilon(k)$$