# One pixel attack for fooling deep neural networks

## 一、概要

### 本文主要提出了一种修改单个像素就可以有效生成AE(Adversarial Example)的方法，这种方法基于微分型遗传算法，是一种低成本、黑箱化的AE生成方法，也为探索NN(Neural Network)分类边界提供了一定的指导。

## 二、简介

### 一般的AE生成算法专注于生成人难以感知，但是能大幅扰乱NN分类的AE。本文算法则探讨其极限状况——生成人能感知，亦能大幅扰乱NN分类的AE。本文采用OP-ATK(One pixel attack)，能够实现黑箱化的攻击，并且将AE对图像的扰动量化为像素个数，有助于分析图像相近性和了解NN判决边界。

## 三、相关工作(略)

## 四、算法设计

### 1、问题描述：我们很容易就将生成AE划归为一个带约束的优化问题，为了进一步简化问题，我们将约束加强到0阶范数——也就是变动像素个数。而对多个像素的修改可以看作是对图像在多个维度上修改(这里还是有争议的，毕竟文中直接将图像像素排列成向量的方式，会丢失很多空间细节)。

### 2、微分型进化算法：更可能找到全局最优；对训练集标签信息要求低；和NN无关。

### 3、具体算法和设置：400个初始生成点(均匀生成)，共用一个迭代方程，亲代和子代竞争存活，并且采用“早停”机制。

## 五、测试和结果(略……太多了，感兴趣自己阅读即可)

## 六、讨论

### 1、小的AE扰动——输出巨大影响(GoodFellow)。
### 2、OP-ATK通过增加进化代数和初始选择可以增强攻击效果，并且在各模型和训练集上具有良好普适性。
### 3、OP-ATK攻击效用不如其它的AE生成算法(简单且极端)，然而目前的AE检测/防御技术依旧无法摆脱原理如NN一样不明的特点。

## 七、未来展望

### 1、还有很多更有效的DE可以运用于AE的生成。
### 2、DE和NN结合，不仅可以优化参数，而且可以改善网络拓扑结构。
### 3、类似的OP-ATK同样可以迁移到其他领域，例如SSP(Speech Signal Processing, 语音信号处理)和NLP(Natural Lamguage Processing, 自然语言处理)
