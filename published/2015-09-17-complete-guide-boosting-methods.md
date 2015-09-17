#基于R语言的梯度推进算法介绍

##简介

通常来说，我们可以从两个方面来提高一个预测模型的准确性：完善特征工程（feature engineering）或是直接使用Boosting算法。通过大量数据科学竞赛的试炼，我们可以发现人们更钟爱于Boosting算法，这是因为和其他方法相比，它在产生类似的结果时往往更加节约时间。

Boosting算法有很多种，比如梯度推进（Gradient Boosting）、XGBoost、AdaBoost、Gentle Boost等等。每一种算法都有自己不同的理论基础，通过对它们进行运用，算法之间细微的差别也能够被我们所察觉。如果你是一个新手，那么太好了，从现在开始，你可以用大约一周的时间来了解和学习这些知识。

在本文中，笔者将会向你介绍梯度推进算法的基本概念及其复杂性，此外，文中还分享了一个关于如何在R语言中对该算法进行实现的例子。

![](http://static.datartisan.com/upload/attachment/2015/09/bpTVLKvL.jpg)


##快问快答

每当谈及Boosting算法，下列两个概念便会频繁的出现：Bagging和Boosting。那么，这两个概念是什么，它们之间究竟有什么区别呢？让我们快速简要地在这里解释一下：

Bagging：对数据进行随机抽样、建立学习算法并且通过简单平均来得到最终概率结论的一种方法。

Boosting：与Bagging类似，但在样本选择方面显得更为聪明一些——在算法进行过程中，对难以进行分类的观测值赋予了越来越大的权重。

我们知道你可能会在这方面产生疑问：什么叫做越来越大？我怎么知道我应该给一个被错分的观测值额外增加多少的权重呢？请保持冷静，我们将在接下来的章节里为你解答。



##从一个简单的例子出发

假设你有一个初始的预测模型M需要进行准确度的提高，你知道这个模型目前的准确度为80%（通过任何形式度量），那么接下来你应该怎么做呢？

有一个方法是，我们可以通过一组新的输入变量来构建一个全新的模型，然后对它们进行集成学习。但是，笔者在此要提出一个更简单的建议，如下所示：

```
Y = M(x) + error
```

如果我们能够观测到误差项并非白噪声，而是与我们的模型输出（Y）有着相同的相关性，那么我们为什么不通过这个误差项来对模型的准确度进行提升呢？比方说：

```
error = G(x) + error2
```

或许，你会发现模型的准确率提高到了一个更高的数字，比如84%。那么下一步让我们对error2进行回归。

```
error2 = H(x) + error3
```
然后我们将上述式子组合起来：

```
Y = M(x) + G(x) + H(x) + error3
```

这样的结果可能会让模型的准确度更进一步，超过84%。如果我们能像这样为三个学习算法找到一个最佳权重分配，

```
Y = alpha * M(x) + beta * G(x) + gamma * H(x) + error4
```

那么，我们可能就构建了一个更好的模型。

上面所述的便是Boosting算法的一个基本原则，当我初次接触到这一理论时，我的脑海中很快地冒出了这两个小问题：

1.我们如何判断回归／分类方程中的误差项是不是白噪声？如果无法判断，我们怎么能用这种算法呢？

2.如果这种算法真的这么强大，我们是不是可以做到接近100％的模型准确度？

接下来，我们将会对这些问题进行解答，但是需要明确的是，Boosting算法的目标对象通常都是一些弱算法，而这些弱算法都不具备只保留白噪声的能力；其次，Boosting有可能导致过度拟合，所以我们必须在合适的点上停止这个算法。



##试着想象一个分类问题

请看下图：

![](http://static.datartisan.com/upload/attachment/2015/09/Yt2xKiqk.png)

从最左侧的图开始看，那条垂直的线表示我们运用算法所构建的分类器，可以发现在这幅图中有3/10的观测值的分类情况是错误的。接着，我们给予那三个被误分的“＋”型的观测值更高的权重，使得它们在构建分类器时的地位非常重要。这样一来，垂直线就直接移动到了接近图形右边界的位置。反复这样的过程之后，我们在通过合适的权重组合将所有的模型进行合并。



##算法的理论基础

我们该如何分配观测值的权重呢？

通常来说，我们从一个均匀分布假设出发，我们把它称为D1，在这里，n个观测值分别被分配了1/n的权重。

步骤1：假设一个α（t）；

步骤2：得到弱分类器h（t）；

步骤3：更新总体分布，

![](http://static.datartisan.com/upload/attachment/2015/09/dBiKtCFv.png)

其中，

![](http://static.datartisan.com/upload/attachment/2015/09/ur0mlvNE.png)

步骤4：再次运用新的总体分布去得到下一个分类器；

觉得步骤3中的数学很可怕吗？让我们来一起击破这种恐惧。首先，我们简单看一下指数里的参数，α表示一种学习率，y是实际的回应值（＋1或－1），而h（x）则是分类器所预测的类别。简单来说，如果分类器预测错了，这个指数的幂就变成了1 *α， 反之则是－1*α。也就是说，如果某观测值在上一次预测中被预测错误，那么它对应的权重可能会增加。那么，接下来该做什么呢？

步骤5：不断重复步骤1-步骤4，直到无法发现任何可以改进的地方；

步骤6：对所有在上面步骤中出现过的分类器或是学习算法进行加权平均，权重如下所示：

![](http://static.datartisan.com/upload/attachment/2015/09/6wZRZUJ2.png)



##案例练习

最近我参加了由Analytics Vidhya组织的在线hackathon活动。为了使变量变换变得容易，在complete_data中我们合并了测试集与训练集中的所有数据。我们将数据导入，并且进行抽样和分类。

```
library(caret)
rm(list=ls())
setwd("C:\\Users\\ts93856\\Desktop\\AV")
library(Metrics)
complete <- read.csv("complete_data.csv", stringsAsFactors = TRUE)
train <- complete[complete$Train == 1,]
score <- complete[complete$Train != 1,]
set.seed(999)
ind <- sample(2, nrow(train), replace=T, prob=c(0.60,0.40))
trainData<-train[ind==1,]
testData <- train[ind==2,]
set.seed(999)
ind1 <- sample(2, nrow(testData), replace=T, prob=c(0.50,0.50))
trainData_ens1<-testData[ind1==1,]
testData_ens1 <- testData[ind1==2,]
table(testData_ens1$Disbursed)[2]/nrow(testData_ens1)
#Response Rate of 9.052%
```

 接下来，就是构建一个梯度推进模型（Gradient Boosting Model）所要做的：

```
fitControl <- trainControl(method = "repeatedcv", number = 4, repeats = 4)
trainData$outcome1 <- ifelse(trainData$Disbursed == 1, "Yes","No")
set.seed(33)
gbmFit1 <- train(as.factor(outcome1) ~ ., data = trainData[,-26], method = "gbm", trControl = fitControl,verbose = FALSE)
gbm_dev <- predict(gbmFit1, trainData,type= "prob")[,2] 
gbm_ITV1 <- predict(gbmFit1, trainData_ens1,type= "prob")[,2] 
gbm_ITV2 <- predict(gbmFit1, testData_ens1,type= "prob")[,2]
auc(trainData$Disbursed,gbm_dev)
auc(trainData_ens1$Disbursed,gbm_ITV1)
auc(testData_ens1$Disbursed,gbm_ITV2)
```

在上述案例中，运行代码后所看到的所有AUC值将会非常接近0.84。我们随时欢迎你对这段代码进行进一步的完善。在这个领域，梯度推进模型（GBM）是最为广泛运用的方法，在未来的文章里，我们可能会对GXBoost等一些更加快捷的Boosting算法进行介绍。



##结束语

笔者曾不止一次见识过Boosting算法的迅捷与高效，在Kaggle或是其他平台的竞赛中，它的得分能力从未令人失望，当然了，也许这要取决于你能够把特征工程（feature engineering）做得多好了。



原文作者：TAVISH SRIVASTAVA 

翻译：SDCry！！！

原文链接：http://www.analyticsvidhya.com/blog/2015/09/complete-guide-boosting-methods/

