# Distillation as a Defense to Adversarial Perturbations against Deep Neural Networks

## 一、概述：

### DNN(Deep Nerual Network)对AE(Adversarial Example)很脆弱，本文采用一种称为DD(Defensive Distillation)的方法来降低AE对DNN的影响，能将误判率从95%降到0.5%，同时能够将梯度降低至原来的1/10 ^ 30；而AE所需的攻击特征数大约要提升800%，攻击更为艰难。

## 二、简介：

### 由于其优秀的表现， DNN被广泛地应用在各行各业中。然而AE对DNN来说是很有效的一种攻击方法，并且防御方法有限，一般都是通过修改模型/针对部分AE进行防御。而蒸馏(Distillation)是将一个DNN训练所得信息来训练另一个DNN，从而起到压缩DNN规模和计算量，实现在微小系统上的智能计算。

### 在这篇文章中，DD能够降低DNN的梯度变化，从而抑制各种针对梯度进行AE生成的各种方法。可以认为，是针对输入对网络进行平滑。

### 本文贡献点：
### 1、阐述了DNN防御AE需要达成的几个条件；
### 2、蒸馏(Distillation)思想的应用；
### 3、能够平滑模型；
### 4、AE攻击成功率下降——能将误判率从95%降到0.5%；
### 5、AE攻击困难上升——同时能够将梯度降低至原来的1/10 ^ 30；而AE所需的攻击特征数大约要提升数倍，攻击更为艰难。

## 三、对抗DNN：
