---
date: "2022-11-16T00:00:00Z"
external_link: ""
image:
  #caption: Photo by rawpixel on Unsplash
  focal_point: Smart
links:
- icon: twitter
  icon_pack: fab
  name: Follow
  url: https://twitter.com/georgecushen
slides: example
summary: 模型有意义R方法太小怎么办.
tags:
- r
- modeling
title: 模型中变量有意义但R方太小如何解释
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""
---
## 1 前言

最近在处理数据的过程中，发现一个很有意思的现象，模型中的变量显示差别有统计学意义，但是，模型的R方太小，仅有0.04，相当于模型只解释4%的变异。如果只是单一纳入某个，似乎可以理解，但是在这个模型中纳入了不少变量（10个左右），仅解释4%有点说不过去，所以在网上查询资料，看看如何解释。本文主要参考此文

[How to Interpret Regression Models that have Significant Variables but a Low R-squared](https://statisticsbyjim.com/regression/low-r-squared-regression/	"How to Interpret Regression Models that have Significant Variables but a Low R-squared")

## 2 基本概念回顾

在过去的学习中，我们常见的套路是：模型的中的变量具有统计学意义，而其R方通常不会太小，至少是>50%。具有统计学意义的变量提示该变量的变化与因变量变化相关，而合理的R方表明模型解释了大部分的因变量的变异来源。但是，真实世界的数据往往不如人意。通过思考，R方过小可能有以下两种情况：

1）研究者研究设计不正确，研究者没有充分利用自身的专业背景设计合理的研究，找到潜在的影响因素；

2）数据本身的变异过大（本文的主要阐述方向，以统计模拟形式呈现）

## 3 统计模拟

本文统计模拟主要通过拟合单变量的线图来说明。尽管是一元回归，但是对于多元回归的解释仍然有效。

考虑以下两个模型

output1 = 44.53 + 2.024*input

output2 = 44.86 + 2.134*input


如果仅看方程，模型非常相似。但是output的R方为14.7%而output2R方为86.5%。为什么会产生如此大的差距呢？

### 3.1 模型拟合图

[数据数据下载链接](https://statisticsbyjim.com/wp-content/uploads/2017/05/HighLowRsquaredData.csv)。废话不多说，上图。

![具有低 R 平方和高变异性数据的模型的拟合线图。](https://i0.wp.com/statisticsbyjim.com/wp-content/uploads/2017/05/flp_highvar.png?resize=576%2C384&is-pending-load=1)

![具有高 R 平方和低变异性数据的模型的拟合线图。](https://i0.wp.com/statisticsbyjim.com/wp-content/uploads/2017/05/flp_lowvar.png?resize=576%2C384&is-pending-load=1)



尽管模型的方程相似，但是模型的图却是肉眼可见的不同。

### 3.2 模型异同比较

#### 3.2.1 回归模型的相似性

模型主要有以下相似性:

1）方程几乎相等：output = 44 +2 *input;

2）input有统计学意义且P<0.001;

3）回归线都提供了无偏拟合，且斜率均为2

**注意：**具有统计学意义的回归系数的解释不会因为R方过小而改变。此外，两模型的预测值相同。

#### 3.2.2 回归模型的差异

模型的相似性都集中在均值上（平均变化和平均预测值）。但是，模型的最大区别是均值的变异程度（即方差）。

回归系数和预测值侧重于均值，而R方侧重于评估回归线周围的数据分散程度。也就是说，给定数据集，回归线周围的变异性越高，R方就越低。

尽管模型均值如此相似，但是围绕预测的变异性（精度）是不同的。此时通过置信区间评估精度，对于多变量回归模型，绘图无法实现，查看预测值的精度是不错的选择。

#### 3.2.3 使用预测区间检查变异程度

预测区间是指在给定自变量情况下单个预测值出现的可能范围。越窄则说明精度越高，大多数软件都可以计算预测区间。

假设输入自变量为10，两模型的预测值如下：

![具有低 R 平方和高变异性数据的回归模型的预测输出。](https://i0.wp.com/statisticsbyjim.com/wp-content/uploads/2017/05/pred_highvar.png?resize=415%2C211&is-pending-load=1)

![具有高 R 平方和低变异性数据的回归模型的预测表。](https://i0.wp.com/statisticsbyjim.com/wp-content/uploads/2017/05/pred_lowvar.png?resize=407%2C208&is-pending-load=1)

正如前文所提，两预测值几乎相等，但是预测区间却大大不同。高变异性（低R方）的预测区间为-500到630。

而低变异的预测区间为-30到60，约为190个单位。

## 4 小结

回顾一下要点：

1）回归系数和拟合值表示均值

2）R方和预测区间表示变异程度

3）无论R方的大小，都可以相同的方式解释有效变量的系数

4）低的R方可以提示不精确的预测值

## 5 多一点思考

如果R方太小，怎么办？通常，首先想到往模型增加更多变量以增加R方。其实是否增加R方有时依据研究目的而定。

如果你能找到合适的预测因子，那么继续增加变量是可行的。但是，对于每个研究领域，都有固有的不可解释的变异。例如，试图预测人类行为的研究通常R方都小于50%。通过数据处理，可能可以强制是的R方更大，但是，代价往往是使得回归系数，P值和R方变得更加不可信。或许，此时使用校正R方或者预测R方可能更加合适。

如果研究者主要对变量之间的关系感兴趣，所幸的是，R方不会否定任何重要变量的重要性。即使R方过小，具有统计学意义的P值仍然提示这些相关关系存在的可能性，并且系数的解释不会改变，通常没有任何理由可以否定这些相关关系的存在。