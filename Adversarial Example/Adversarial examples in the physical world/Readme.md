# Adversarial examples in the physical world

## 一、概述

### AE(Adversarial Examples)的存在对于许多ML(Machine Learning)分类器是一个巨大的威胁，它们往往能用小到人眼无法察觉的扰动来使分类器得出的结果南辕北辙。但是之前的工作都是基于直接在样本上改动，如果在现实生活中进行变换，结果会如何呢？本文采用ImageNet作为攻击对象，发现即使在现实生活中，变换后的AE仍能对其造成不小的威胁。

## 二、简介

### ML(Machine Learning)分类器在很多领域有着出类拔萃的效果，这是一个不争的事实；但是其对于AE的脆弱性同样不容忽视。这种AE所加的扰动往往人眼不可察，但是却能使得ML分类器屡屡犯错。同时，AE还对不同ML和不同训练集具有普适性，这就意味着对于ML分类器的黑箱攻击是可能的。
### 然而之前的AE生成模型都是直接在数据上操作，而缺少在现实生活中AE的实例。之前的工作要不就是没有在现实生活中进行，要不就是扰动太大不足以称为AE。(当然和它相近的有一篇Sharif,2016的论文，比这篇论文更为精细)
### 本文采用的是白箱攻击(ML分类器的参数已知)，直接用相机拍摄生成AE所打印的照片作为分类器输入，之后也会对这种变换做一定的理论解释。

## 三、生成AE的方法

### 1、Fast Method:无需迭代，利用L∞范数求解，速度快但粗糙；
### 2、Basic 迭代法:Fast Method迭代版，每次扰动更小，更精细化；
### 3、最小似然迭代法：Basic 迭代法指定攻击到最小可能的类别，误判率极高。

### 实验效果：
### 1、受到AE攻击后判断准确率均明显下降，TOP5比TOP1稍好；
### 2、Fast Method在ε小的时候效果没啥区别，当ε大的时候虽然攻击效果更好，但是扰动出来的图片神鬼莫测。
### 3、最小似然迭代法效果最好，在ε很小的时候就拥有最强的攻击性。
### 4、Basic 迭代法在ε到达一定程度后不再变化，性能先居中后最差。
### 5、ε小的情况没有细加比较，因为谁上谁都能行。

## 四、现实中的AE

### 1、AE图像重建率：
### AE图像重建率 = （原图像分类正确&AE攻击成功&转换后AE攻击失效）的图像数 / （原图像分类正确&AE攻击成功）的图像数

### 2、实验步骤（白箱攻击）：
### 基本实验步骤：生成AE——打印AE——拍摄AE——输入AE——判断效果
### 实验环境：普通室内的灯光、正对拍摄
### 测试集划分：
### 正常情况：给定ε和AE生成方法，随机给定分类正确的样本对其进行攻击，攻击到分类失败为止；
### 预处理情况：给定符合“正常情况”的，经过预处理样本，攻击者可以随意选择样本来攻击。

### 3、实验结果
### Fast Method虽然效果略差，但是是最稳健的，可能是因为更微小的扰动在拍摄时可能失效。
### AE重建率在“预处理情况”下可能比“正常情况”更高（尤其是采用迭代的方法），这更加说明了迭代算法施加的更微小的扰动在拍摄状态下可能失真。

### 4、黑箱攻击（也可行）

## 五、人为图像变换

### 5种人为变换——对比度/亮度改变、高斯模糊、高斯噪声、JPEG编码；
### 对每个样本用各种AE生成方法后，再用所有的变换扰动一遍，最后计算重构率。
### 实验结果：
### 1、FGSM最为稳健，LL最不稳健；
### 2、TOP5重建率>TOP1重建率，因为TOP5->TOP5总比TOP1->TOP1简单些；
### 3、对比度/亮度改变对AE影响较小；高斯模糊、高斯噪声、JPEG编码AE影响较大(80%~90%)，不过没有一个到100%的。

## 六、展望

### 不同物体(除了图像之外)？不同ML模型？黑箱攻击？更高成功率？如何对抗AE?
