#线性回归模型：预测发电站能源输出
本文示范如何拟合一个线性回归模型来预测某联合循环发电站的能源输出。所使用的数据集来自 UCI Machine Learning Repository。此数据集包含五列：环境温度 (AT)，环境压强 (AP)， 相对湿度(RH)，真空排气 (EV) 和发电站的每小时电力净输出量 (PE)。前四列为特征变量，用来预测输出量(PE) 。

##读取和分割数据
数据是xlsx格式，所以需要用xlsx包（译者注：本实例是基于R语言）。这里我们使用原文件第一张工作表的数据。
![](http://static.datartisan.com/upload/attachment/2015/10/5WWqUONx.png)
接下来，需要把数据分成训练集和测试集。从名字可以看出，训练集是用来训练和建立模型，之后测试集来测试模型。假设使用75%的数据作为训练集，25%的数据作为测试集。可以用下列步骤实现：
![](http://static.datartisan.com/upload/attachment/2015/10/brUWFZIa.png)
这里解释一下以上各条命令的作用。
首先，设定种子以便结果可被重现。 
然后，创造一个数列。数列的长度等于数据集的行数。这些数字作为数据集的序数。我们随机选择数列中75%的数字，并储存为变量split。
最后，把数据集里对应着split里序数的行复制为训练数据trainData，剩下的行作为测试数据testData。

##建立预测模型
现在让我们开始建立预测模型。这里使用R语言的lm函数。
predictionModel <- lm(PE ~ AT + V + AP + RH, data = trainData) 
上面的函数会试图根据AT, V, AP, RH预测PE。我们使用了数据集里所有的变量，所以上面这条命令可以简写（这种写法在变量数量较多时很有用）：
predictionModel <- lm(PE ~ ., data = trainData) 
现在我们可以查看数据集的总结。

![](http://static.datartisan.com/upload/attachment/2015/10/pjy6v8id.png)
这可以帮助我们决定要在模型中包含哪个变量。线性回归能由如下等式表示： y_i = β_1 x_i1 + β_2 x_i2 + β_3 x_i3 + … + ε_i 。这里y_i 代表我们想预测的输出(PE) ，x_i代表众多属性变量 (AT, V, AP, and RH)， β 代表它们的系数，ε 代表常量。

总结summary的第一列，即Estimate有多个值。第一个值对应 ε，其他对应各个β值。如果某个变量的系数是0或趋近0，则意味着它对预测影响很小或没有影响，因此可以删除。标准差standard error 展示在多大程度上系数根据Estimate值变化。t value是由Estimate除以标准差得到。最后一列是测量系数多大程度上可能为0，它与t value成反比。因此，理想的变量应该是t value的绝对值高或Pr(>|t|) 的绝对值低。

确定哪个变量更重要，最简便的方法是看它旁边的星号数量。表格底部对此进行了解释。三个星号的变量最重要，其次两星，再次一星。一个点表示变量可能重要可能不重要，通常不被纳入模型中，没有任何标识的变量不重要。

在我们的模型中，可以看到所有的变量都很重要，所以模型保留原样。如果你处理的数据集中，有一个或多个变量不重要，建议每次试探性删除仅一个变量。因为当两个变量彼此高度相关，它们一起可能对模型不重要。但当其中一个被移除，另一个可能变得重要。这是由于存在多重共线性。点击这里这里了解多重共线性。

检查模型准确度最简便的方法是查看R-squared。上面的summary 提供了两个R-squared 值：Multiple R-squared 和 Adjusted R-squared。Multiple R-squared 计算方式如下：
Multiple R-squared = 1 – SSE/SST 
这里的SSE是残差的平方和。残差是预测值和实际值的差，可以通过predictionModel$residuals得到。SST是实际值和平均值之间的差的平方和。
假设我们有实际值 5, 6, 7, 8。 模型的预测值是4.5, 6.3, 7.2, 7.9。那么， SSE = (5 – 4.5) ^ 2 + (6 – 6.3) ^ 2 + (7 – 7.2) ^ 2 + (8 – 7.9) ^ 2; SST算作: mean = (5 + 6 + 7 + 8) / 4 = 6.5; SST = (5 – 6.5) ^ 2 + (6 – 6.5) ^ 2 + (7 – 6.5) ^ 2 + (8 – 6.5) ^ 2
Adjusted R-squared与Multiple R-squared类似，但前者把变量数纳入考虑。这意味着每当预测模型加入一个新变量，Multiple R-squared都会增加，但如果此变量不重要Adjusted R-squared会减小。点击 这里了解更多。
R-squared等于1 意味着这是个完美的预测模型。R-squared等于0意味着此模型跟基线模型没什么区别（基线模型的预测值永远等于平均值）。从上面的summary中，可以看出R-squared值很高，等于0.9284。

##测试模型
现在我们来对测试数据使用此模型。
prediction <- predict(predictionModel, newdata = testData) 
查看几个预测值，跟测试集里的实际PE值作比较。
head(prediction)         
2        4       12       13       14       17  
444.0433 450.5260 456.5837 438.7872 443.1039 463.7809   

head(testData$PE)  
[1] 444.37 446.48 453.99 440.29 451.28 467.54 
实际值444.37，预测值444.0433。实际值446.48，预测值450.5260等等。
我们可以使用如下方法计算预测模型在测试数据上的R-squared值：
SSE <- sum((testData$PE - prediction) ^ 2) 
SST <- sum((testData$PE - mean(testData$PE)) ^ 2) 
1 - SSE/SST  0.9294734 
本实例到此结束。希望你喜欢，也希望它对你有用！如有任何问题，请给我留言或在Twitter上联系我。


原文作者:Teja Kodali
翻译:王鹏宇
