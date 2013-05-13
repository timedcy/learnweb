---
published: true
layout: post
title: 神经网络
description: "Neural networks"
category: 神经网络
tags: [神经网络, MLP, Backpropagation]
---

{% include JB/setup %}

##Neural networks的历史
* 1943年，McCulloch和Pitts建立了neuron的数学模型，$$ y=I\left(\Sigma _{i} w_{i} x_{i}>\theta \right) $$
* 1957年，Frank Rosenblatt发明perceptron learning algorithm
* 1969年，Bryson, A. 和Y.-C. Ho发现backpropagation algorithm
* 1986年，Rumelhart、Hinton和Williams发现backpropagation algorithm可以拟合有隐藏层的模型，并引起人们注意
* 1987年，Sejnowski和Rosenberg发明NETtalk，通过隐藏层将单词映射成语音元素，从而可以人工合成语音
* 1989年， Yann Le Cun等发明LeNet系统
* 1992年，Boser发明support vector machine，从此kernel方法流行
* 2002年，Geoff Hinton发明contrastive divergence training procedure（每层依次训练），首次可以处理深层网络，从而神经网络又流行

##Feedforward neural network
Feedforward neural network，或称multi-layer perceptron (MLP)，是logistic regression模型逐层叠加并在输出层用logistic regression或linear regression来实现的。

$$
\begin{equation}
\begin{array}{rl}
p\left( y | \mathbf{x}, \mathbf{\theta} \right) &= \mathcal{N} \left(y| \mathbf{w}^{T} \mathbf{z} \left( \mathbf{x} \right), \sigma^2 \right) \\
\mathbf{z} \left( \mathbf{x} \right) &= g \left( \mathbf{V} \mathbf{x} \right) = \left[ g \left( \mathbf{v}_{i}^{T} \mathbf{x} \right), ..., g \left( \mathbf{v}_{H}^{T} \mathbf{x} \right) \right]
\end{array}
\label{eq:MLP}
\end{equation}
$$

$$ \eqref{eq:MLP} $$中$$ g $$是非线性activation或称transfer function (通常是logistic function)，$$ \mathbf{z} \left( \mathbf{x} \right) = \mathbf{\phi} \left( \mathbf{x}, \mathbf{V} \right) $$称hidden layer，$$ H $$是hidden units的数量，$$ \mathbf{V} $$是输入到隐藏层的权重矩阵，$$ \mathbf{w} $$是隐藏层到输出层的权重向量。如下图所示：
<CENTER><p><img src="{{ site.img_url }}/A neural network with one hidden layer.png" alt="A neural network with one hidden layer" /></p></CENTER>

##Backpropagation algorithm

不像广义线性模型(GLM)，MLP的NLL(negative log likelihood)是non-convex函数，但是可以通过gradient-based optimization方法找到局部的最优值。
MLP通常参数量大，而且训练数据量大，所以一般采用first-order online methods去训练，其又称为stochastic gradient descent方法。而GLM通常用IRLS(Iteratively reweighted least squares)来解，或者称second-order offline method。

Backpropagation算法根据微积分的链式法则求这个NLL的梯度向量。令第$$ n $$个输入样本为$$ \mathbf{x}_{n} $$，则隐藏层合成前$$ \mathbf{a}_{n}=\mathbf{V} \mathbf{x}_{n} $$，合成后激烈输出层为$$ \mathbf{z}_{n} =  g \left( \mathbf{a}_{n} \right) $$， $$ g $$是某种transfer function。而隐藏层到输出层则激励前加权层为$$ \mathbf{b}_{n}=\mathbf{W} \mathbf{z}_{n} $$，激励后输出层为$$ \mathbf{\hat{y}}_{n} = h \mathbf{b}_{n} $$。连接起来就是：

$$
\begin{equation}
\mathbf{x}_{n} \stackrel{\mathbf V}{\longrightarrow} 
\mathbf{a}_{n} \stackrel{g}{\longrightarrow} 
\mathbf{z}_{n} \stackrel{\mathbf W}{\longrightarrow} 
\mathbf{b}_{n} \stackrel{h}{\longrightarrow} 
\mathbf{\hat y}_{n} 
\end{equation}
$$

模型的参数是$$ \mathbf{\theta} = \left( \mathbf{V}, \mathbf{W} \right) $$，第一层和第二层的权重矩阵构成。对于$$K$$个输出值的NLL在regression用squared error表示$$ \eqref{eq:MLPNLLEqReg} $$，在classification用cross entropy表示$$ \eqref{eq:MLPNLLEqClf} $$。

$$
\begin{equation}
J(\theta) = - \sum_{n} \sum_{k} {\left( \hat{y}_{nk} (\theta) - y_{nk} \right)}^2
\label{eq:MLPNLLEqReg}
\end{equation}
$$

$$
\begin{equation}
J(\theta) = - \sum_{n} \sum_{k} y_{nk} \log {\hat{y}_{nk} (\theta) }
\label{eq:MLPNLLEqClf}
\end{equation}
$$

以上式子求极小值需求$$ \nabla _{\theta}J$$。对每$$ n $$个单独的样本通过求其gradient的总和来得到，对输出层考虑$$ b_{nk}=\mathbf{w}_{k}^{T} \mathbf{z}_n$$有：

$$
\begin{equation}
 \nabla _{\mathbf{w_k}}J_n = \frac{\partial J_n}{\partial b_{nk}} \nabla _{\mathbf{w_k}}b_{nk} = \frac{\partial J_n}{\partial b_{nk}} \mathbf{z}_{n}
\end{equation}
$$

