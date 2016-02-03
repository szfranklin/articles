#用 Python 做数据处理必看：12 个使效率倍增的 Pandas 技巧（上）

##导语

Python正迅速成为数据科学家偏爱的语言，这合情合理。它拥有作为一种编程语言广阔的生态环境以及众多优秀的科学计算库。如果你刚开始学习Python，可以先了解一下Python的学习路线。
在众多的科学计算库中，我认为Pandas对数据科学运算最有用。Pandas，加上Scikit-learn几乎能构成了数据科学家所需的全部工具。 本文旨在提供Python数据处理的12种方法。文中也分享了一些会让你的工作更加便捷的小技巧。
在继续推进之前，我推荐读者阅览一些关于数据探索 (data exploration)的代码。
为了帮助理解，本文用一个具体的数据集进行运算和操作。本文使用了贷款预测(loan prediction) 问题数据集，下载数据集请到http://datahack.analyticsvidhya.com/contest/practice-problem-loan-prediction。

##开始工作

首先我要导入要用的模块，并把数据集载入Python环境。

```
import pandas as pd

import numpy as np

data = pd.read_csv("train.csv", index_col="Loan_ID")
```

##1.布尔索引(Boolean Indexing)

如何你想用基于某些列的条件筛选另一列的值，你会怎么做？例如，我们想要一个全部无大学学历但有贷款的女性列表。这里可以使用布尔索引。代码如下：

```
data.loc[(data["Gender"]=="Female") & (data["Education"]=="Not Graduate") & (data["Loan_Status"]=="Y"), ["Gender","Education","Loan_Status"]]
​
```

想了解更多请阅读 Pandas Selecting and Indexing

##2.Apply函数

Apply是摆弄数据和创造新变量时常用的一个函数。Apply把函数应用于数据框的特定行/列之后返回一些值。这里的函数既可以是系统自带的也可以是用户定义的。例如，此处可以用它来寻找每行每列的缺失值个数：

```
#创建一个新函数:

def num_missing(x):

  return sum(x.isnull())

#Apply到每一列:

print "Missing values per column:"

print data.apply(num_missing, axis=0) #axis=0代表函数应用于每一列

#Apply到每一行:

print "\nMissing values per row:"

print data.apply(num_missing, axis=1).head() #axis=1代表函数应用于每一行
```

输出结果：

由此我们得到了想要的结果。
注意：第二个输出使用了head()函数，因为数据包含太多行。
想了解更多请阅读 Pandas Reference (apply)

##3.替换缺失值

‘fillna()’ 可以一次解决这个问题。它被用来把缺失值替换为所在列的平均值/众数/中位数。

```
#首先导入一个寻找众数的函数：

from scipy.stats import mode

mode(data['Gender'])
```

输出: ModeResult(mode=array([‘Male’], dtype=object), count=array([489]))
返回了众数及其出现次数。记住，众数可以是个数组，因为高频的值可能不只一个。我们通常默认使用第一个：

```
mode(data['Gender']).mode[0]
```

现在可以填补缺失值，并用上一步的技巧来检验。

```
#值替换:

data['Gender'].fillna(mode(data['Gender']).mode[0], inplace=True)

data['Married'].fillna(mode(data['Married']).mode[0], inplace=True)

data['Self_Employed'].fillna(mode(data['Self_Employed']).mode[0], inplace=True)

#再次检查缺失值以确认:

print data.apply(num_missing, axis=0)
```

由此可见，缺失值确定被替换了。请注意这是最基本的替换方式，其他更复杂的技术，如为缺失值建模、用分组平均数（平均值/众数/中位数）填充，会在今后的文章提到。
想了解更多请阅读 Pandas Reference (fillna)

##4.透视表

Pandas可以用来创建 Excel式的透视表。例如，“LoanAmount”这个重要的列有缺失值。我们可以用根据 ‘Gender’、‘Married’、‘Self_Employed’分组后的各组的均值来替换缺失值。每个组的 ‘LoanAmount’可以用如下方法确定：

```
#Determine pivot table

impute_grps = data.pivot_table(values=["LoanAmount"], index=["Gender","Married","Self_Employed"], aggfunc=np.mean)

print impute_grps
```

想了解更多请阅读 Pandas Reference (Pivot Table)

##5.多重索引

你可能注意到上一步骤的输出有个奇怪的性质。每个索引都是由三个值组合而成。这叫做多重索引。它可以帮助运算快速进行。
延续上面的例子，现在我们有了每个分组的值，但还没有替换。这个任务可以用现在学过的多个技巧共同完成。

```
#只在带有缺失值的行中迭代：

for i,row in data.loc[data['LoanAmount'].isnull(),:].iterrows():

  ind = tuple([row['Gender'],row['Married'],row['Self_Employed']])

  data.loc[i,'LoanAmount'] = impute_grps.loc[ind].values[0]

#再次检查缺失值以确认：

print data.apply(num_missing, axis=0)
```

注：

多重索引需要在loc中用到定义分组group的元组(tuple)。这个元组会在函数中使用。
需要使用.values[0]后缀。因为默认情况下元素返回的顺序与原数据库不匹配。在这种情况下，直接指派会返回错误。

##6. 二维表

这个功能可被用来获取关于数据的初始“印象”（观察）。这里我们可以验证一些基本假设。例如，本例中“Credit_History” 被认为对欠款状态有显著影响。可以用下面这个二维表进行验证：

```
pd.crosstab(data["Credit_History"],data["Loan_Status"],margins=True)
```

这些数字是绝对数值。不过，百分比数字更有助于快速了解数据。我们可以用apply函数达到目的：

```
def percConvert(ser):

  return ser/float(ser[-1])

  pd.crosstab(data["Credit_History"],data["Loan_Status"],margins=True).apply(percConvert, axis=1)
```

现在可以很明显地看出，有信用记录的人获得贷款的可能性更高：有信用记录的人有80% 获得了贷款，没有信用记录的人只有 9% 获得了贷款。
但不仅仅是这样，其中还包含着更多信息。由于我现在知道了有信用记录与否非常重要，如果用信用记录来预测是否会获得贷款会怎样？令人惊讶的是，在614次试验中我们能预测正确460次，足足有75%！
如果此刻你在纳闷，我们要统计模型有什么用，我不会怪你。但相信我，在此基础上提高0.001%的准确率都是充满挑战性的。你是否愿意接受这个挑战？
注：对训练集而言是75% 。在测试集上有些不同，但结果相近。同时，我希望这个例子能让人明白，为什么提高0.05% 的正确率就能在Kaggle排行榜上跳升500个名次。
想了解更多请阅读Pandas Reference (crosstab)

感谢您阅读到这里，在下一篇文章中将继续为您介绍其余六个实用技巧，请持续关注数据工匠。
原作者：AARSHAY JAIN 
翻译：王鹏宇
原文地址：
http://www.analyticsvidhya.com/blog/2016/01/12-pandas-techniques-python-data-manipulation/
