# The Limitations of Deep Learning in Adversarial Settings

## 一、概述

### 这篇文章是EuroS&P2016的文章，提出了对于NN(Neural Network)进行对抗样本生成的具体分类，介绍了一种生成AE(Adversarial Example)的方法，并分析了对分类器各个分类攻击成功的难易程度，最后讨论了对AE的防御办法。

## 二、介绍

### 主要就是总结了一下前人的工作，一般都是采用梯度来进行AE的生成，而本文重点是：
### 1、构建了对攻击难易程度和普适程度的二维空间，用来衡量AE对抗效果的强弱。
### 2、利用前向微分而非梯度进行AE生成，构建AE特征图来解释AE边界。
### 3、分析AE特点，探讨常用AE的防御方法。

## 三、对于DL(Deep Learning)攻击模型的分类
