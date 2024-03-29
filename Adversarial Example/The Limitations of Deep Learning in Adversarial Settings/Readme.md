# The Limitations of Deep Learning in Adversarial Settings

## 一、概述

### 这篇文章是EuroS&P2016的文章，提出了对于NN(Neural Network)进行对抗样本生成的具体分类，介绍了一种生成AE(Adversarial Example)的方法，并分析了对分类器各个分类攻击成功的难易程度，最后讨论了对AE的防御办法。

## 二、介绍

### 主要就是总结了一下前人的工作，一般都是采用梯度来进行AE的生成，而本文重点是：
### 1、构建了对攻击难易程度和普适程度的二维空间，用来衡量AE对抗效果的强弱。
### 2、利用前向微分而非梯度进行AE生成，构建AE特征图来解释AE边界。
### 3、分析AE特点，探讨常用AE的防御方法。

## 三、对于DL(Deep Learning)攻击模型的分类
### 主要分为两个维度：攻击目的&攻击所需要的信息
### 1、攻击目的：攻击强度由低到高分为
### (1)降低分类置信度
### (2)误分类
### (3)指定类别的误分类
### (4)对于任意输入，都能实现指定类别的误分类

### 2、攻击所需要的信息：信息量从大到小分为
### (1)已知训练集及网络结构
### (2)仅知网络结构
### (3)仅知训练集
### (4)仅可使用网络
### (5)仅知某些输入输出的对应关系

### 本文所述AE生成的属于高攻击强度、高信息量的方法(白箱攻击)

## 四、AE生成方法

### 1、简单NN例子——构建对于AND函数的攻击AE
###  一般都可以将AE的生成问题化归为优化问题，但是如何找寻合适的扰动是一个比较麻烦的问题。本文基于对网络函数F对输入特征的前向导数，寻求前向导数大的作为寻找优化的搜索方向。
### 可以发现，合适的小扰动可以引起分类的截然不同；前向导数往往可以指示如何缩小搜索范围；不是所有的类别都能成功扰动至另一类别。

### 2、适用到深层前馈网络——AE生成算法1.0
### 给定一些参数意义之后，就是一个简要的AE生成算法，后层的前向导数可以划归为一个递推问题，所以说计算方面还好。
### 算法中主要涉及到一个Saliency Map(SM)的计算，这个SM的计算公式已经成现在文中，直观上来看，它就是反应改动哪个输入特征对归到某一类有多大的帮助，由此我们可以获取到更好的扰动生成方法；同样的如果简单取反，就是我们应当对那些输入谨慎处理。(防御策略)
### 算法中人为参数只有扰动范围和每次扰动大小是人为确定的，将在之后章节详述。

## 五、实验

### LeNet在MNIST数据集上进行手写数据的识别——生成AE。
### 1、AE生成算法
### 在前面算法的基础上进行修改，其中每次修改量θ，最大改动幅度γ为人为给定，并采取两种不同的修改策略：加性噪声/减性噪声。

### 2、加性噪声
### 实验：将每个数字修改至任意其他数字，θ=1，γ=∞（每次修改2个像素，由于改多了计算量组合爆炸，该少了有些改动不起反应）；并且用最后一层Hidden Layer结果代替输出层结果更好（输出层对结果做了归一，有损精度和范围）。
### 算法：跟之前阐述的SM算法基本相同，只是更为具体。

### 3、减性噪声
### 实验：将每个数字修改至任意其他数字，θ=-1，γ=∞其它和加性噪声基本相同。
### 特点：减性噪声的扰动对人眼来说更为隐蔽。

## 六、结果

### 目标：
### 任何样本都能被攻击？
### 如何辨识更容易被攻击的样本？
### 人和NN对AE的反应有何差异？(4.02%扰动过后NN误判率97.10%，14.29%扰动过后人还能保持不误判)

### 1、任何样本都能被攻击？——“不一定”，AE攻击大数据分析：
### 当最大改动幅度γ足够大时，攻击成功率不受其约束（因为有些样本用尽最大攻击范围也改不了）
### 加性噪声攻击 比 减性噪声攻击 更有效(97.10% > 64.7%)(可能是因为减性噪声在减少pixel的同时降低了信息熵，使得DNN难提高置信度，所以比较难误判)(个人认为有些其他解释，因为减少pixel不一定造成信息熵下降，反之亦然，从图形上看，倒有可能是减少pixel而降低连通性造成的)

### 2、如何辨识更容易被攻击的样本？
### 如果从结果来分析(后验分析)，采用攻击成功率和攻击成功平均扰动图就能够清晰地获知相应信息，攻击成功率越高，攻击成功平均扰动越小，攻击就越容易成功。
### 如果从一开始分析(先验分析)，采用“困难度量”是一个从数学上来说容易理解的做法，它通过计算 攻击成功平均扰动 在 攻击成功率上的积分(实际采用累加来近似求解)作为攻击的“困难度量”。(然而不太有用)
### 采用“对抗距离”可能是一个更好的方法：“对抗距离”大致和对应SM的非零元个数(L0范数)呈线性关系，能够更好的预测。
### 对一个网络F的稳健性，建议使用其最容易攻击的输入和类别进行定义(木桶效应)。

## 七、讨论

### 本文方法同样适合无监督学习，但是由于没有标签，所以只能足够接近网络对应输出。
### 不适用于RNN，因为利用前向导数如果有循环就不能如此简单计算。
### 利用SM图攻击很快，每个AE生成<1s。
### 防御方法：
### (1)AE检测：利用文中所述方法(后验)
### (2)提升NN稳健性：GAN/AE辅助训练

