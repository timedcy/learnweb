---
published: false
layout: post
title: "机器学习中的常用算法简介"
description: ""
category: 研究学术
tags: [模式识别, 机器学习, 算法, SVD, NMF, PCA, LDA, ICA, LSA, pLSA, LSI, EM, logistic回归, 概率图模型（PGM）]
---
{% include JB/setup %}

##矩阵的奇异值分解（SVD）
奇异值分解的R代码片段如下：
{% highlight r %}

v <- c(
1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,
1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,
1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,
1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,
1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,
1,1,0,0,0,0,0,0,0,0,0,0,0,1,1,
1,1,0,0,0,0,0,0,0,0,0,0,0,1,1,
1,1,0,0,0,0,0,0,0,0,0,0,0,1,1,
1,1,0,0,0,1,1,1,1,1,0,0,0,1,1,
1,1,0,0,0,1,1,1,1,1,0,0,0,1,1,
1,1,0,0,0,1,1,1,1,1,0,0,0,1,1,
1,1,0,0,0,1,1,1,1,1,0,0,0,1,1,
1,1,0,0,0,1,1,1,1,1,0,0,0,1,1,
1,1,0,0,0,1,1,1,1,1,0,0,0,1,1,
1,1,0,0,0,1,1,1,1,1,0,0,0,1,1,
1,1,0,0,0,1,1,1,1,1,0,0,0,1,1,
1,1,0,0,0,1,1,1,1,1,0,0,0,1,1,
1,1,0,0,0,0,0,0,0,0,0,0,0,1,1,
1,1,0,0,0,0,0,0,0,0,0,0,0,1,1,
1,1,0,0,0,0,0,0,0,0,0,0,0,1,1,
1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,
1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,
1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,
1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,
1,1,1,1,1,1,1,1,1,1,1,1,1,1,1
)
m <- matrix(v, 15, 25)

svd(m)

{% endhighlight %}

###参考资料
[SVD分解的理解](http://www.bfcat.com/index.php/2012/03/svd-tutorial/)    
[机器学习相关——SVD分解](http://www.cnblogs.com/luchen927/archive/2012/01/19/2321934.html)

##矩阵的非负因子分解（NMF）
NMF的基本思想可以简单描述为：对于任意给定的一个非负矩阵A，NMF算法能够寻找到一个非负矩阵U和一个非负矩阵V，使得满足，从而将一个非负的矩阵分解为左右两个非负矩阵的乘积。

###参考资料
汪鹏：《非负矩阵分解：数学的奇妙力量》（计算机教育）    

##logistic回归
Logistic Regression 就是一个被logistic方程归一化后的线性回归。

##协同过滤（CF，Collaborative Filtering）
要理解什么是协同过滤 (Collaborative Filtering, 简称CF)，首先想一个简单的问题，如果你现在想看个电影，但你不知道具体看哪部，你会怎么做？大部分的人会问问周围的朋友，看看最近有什么好看的电影推荐，而我们一般更倾向于从口味比较类似的朋友那里得到推荐。这就是协同过滤的核心思想。最典型的CF的两个分支试基于用户的CF和基于物品的CF。

基于用户的CF的基本思想相当简单，基于用户对物品的偏好找到相邻邻居用户，然后将邻居用户喜欢的推荐给当前用户。计算上，就是将一个用户对所有物品的偏好作为一个向量来计算用户之间的相似度，找到K邻居后，根据邻居的相似度权重以及他们对物品的偏好，预测当前用户没有偏好的未涉及物品，计算得到一个排序的物品列表作为推荐。例如：对于用户A，根据用户的历史偏好，这里只计算得到一个邻居用户C，然后将用户C喜欢的物品D推荐给用户A。

基于物品的CF的原理和基于用户的CF类似，只是在计算邻居时采用物品本身，而不是从用户的角度，即基于用户对物品的偏好找到相似的物品，然后根据用户的历史偏好，推荐相似的物品给他。从计算的角度看，就是将所有用户对某个物品的偏好作为一个向量来计算物品之间的相似度，得到物品的相似物品后，根据用户历史的偏好预测当前用户还没有表示偏好的物品，计算得到一个排序的物品列表作为推荐。例如：对于物品A，根据所有用户的历史偏好，喜欢物品A的用户都喜欢物品C，得出物品A和物品C比较相似，而用户C喜欢物品A，那么可以推断出用户C可能也喜欢物品C。

###参考资料
[探索推荐引擎内部的秘密，第 2 部分: 深入推荐引擎相关算法 - 协同过滤](http://www.ibm.com/developerworks/cn/web/1103_zhaoct_recommstudy2/index.html)

##强化学习（RL，Reinforcement Learning）
训练分类器的典型做法是，给定一个输入样本，计算它的输出类别，把它与已知的类别标记作比较，根据差异来改善分类器的性能。在强化学习或给予评价的学习（Learning With a Critic）中，并不需要指明目标类别的教师信号。相反的，它只需要教师对这次分类任务完成情况给出对或错的反馈，而不需要指出错在哪里。

强化学习的思想来自于条件反射理论和动物学习理论，它是受到动物学习过程启发而得到的一种仿生算法。Agent通过对感知到的环境状态采取各种试探动作，获得环境状态的适合度评价值（通常是一个奖励或惩罚信号），从而修改自身的动作策略以获得较大的奖励或较小的惩罚，强化学习就是这样一种赋予Agent学习自适应性能力的方法。

有影响的强化学习算法有TD算法、Q学习算法、Sarsa算法、Dyan算法、R学习算法和H学习等。

###参考资料
Richard O. Duda等：《模式分类》   
黄炳强等：[《强化学习原理、算法及应用》](http://file.lw23.com/c/c2/c26/c263886a-152d-4167-9849-251f6a8ec2d4.pdf)（河北工业大学学报）


##综合参考文献
[The Most Important Algorithms](http://www.risc.jku.at/people/ckoutsch/stuff/e_algorithms.html)    
[探索推荐引擎内部的秘密](http://www.ibm.com/developerworks/cn/views/web/libraryview.jsp?view_by=search&sort_by=Date&sort_order=desc&view_by=Search&search_by=探索推荐引擎内部的秘密&dwsearch.x=12&dwsearch.y=11&dwsearch=Go)    
[CS 229 Machine Learning Course Materials](http://cs229.stanford.edu/materials.html)    
[A Programmer's Guide to Data Mining](http://guidetodatamining.com)   
[程序员面试、算法研究、编程艺术、红黑树、数据挖掘5大系列集锦](http://blog.csdn.net/v_JULY_v/article/details/6543438)    
[leftnoteasy（关注于 机器学习、数据挖掘、并行计算、数学）](http://www.cnblogs.com/LeftNotEasy/)     
[Forever Album 的机器学习专栏](http://foreveralbum.yo2.cn/articles/category/机器学习)    
[zrq-Sophia的专栏 的Machine Leaning专栏](http://blog.csdn.net/abcjennifer/article/category/1173803)    
[Reflections 的Machine Leaning专栏](http://tech.bobgo.net/?cat=4)    
[淮静的博客 的机器学习专栏](http://blog.163.com/huai_jing@126/blog/#m=0&t=1&c=fks_084065080082085074081080095095085081080075082087095075087)   
[行走的馒头](http://zhfuzh.blog.163.com)
