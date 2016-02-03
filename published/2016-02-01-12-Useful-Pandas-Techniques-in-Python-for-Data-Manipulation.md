#用 Python 做数据处理必看：12 个使效率倍增的 Pandas 技巧（下） 

##7 – 数据框合并

当我们有收集自不同来源的数据时，合并数据框就变得至关重要。假设对于不同的房产类型，我们有不同的房屋均价数据。让我们定义这样一个数据框：

```
prop_rates = pd.DataFrame([1000, 5000, 12000], index=['Rural','Semiurban','Urban'],columns=['rates'])

prop_rates
```

现在可以把它与原始数据框合并：

```
data_merged = data.merge(right=prop_rates, how='inner',left_on='Property_Area',right_index=True, sort=False)

data_merged.pivot_table(values='Credit_History',index=['Property_Area','rates'], aggfunc=len)
```

这张透视表验证了合并成功。注意这里的 ‘values’无关紧要，因为我们只是单纯计数。
想了解更多请阅读Pandas Reference (merge)

##8 – 给数据框排序

Pandas可以轻松基于多列排序。方法如下：

```
data_sorted = data.sort_values(['ApplicantIncome','CoapplicantIncome'], ascending=False)

data_sorted[['ApplicantIncome','CoapplicantIncome']].head(10)
```

注：Pandas 的“sort”函数现在已经不推荐使用，我们用 “sort_values”函数代替。
想了解更多请阅读Pandas Reference (sort_values)

##9 – 绘图（箱型图&直方图）

许多人可能没意识到Pandas可以直接绘制箱型图和直方图，不必单独调用matplotlib。只需要一行代码。举例来说，如果我们想根据贷款状态Loan_Status来比较申请者收入ApplicantIncome：

```
data.boxplot(column="ApplicantIncome",by="Loan_Status")



data.hist(column="ApplicantIncome",by="Loan_Status",bins=30)
```

可以看出获得/未获得贷款的人没有明显的收入差异，即收入不是决定性因素。
想了解更多请阅读Pandas Reference (hist) | Pandas Reference (boxplot)

10 – 用Cut函数分箱

有时把数值聚集在一起更有意义。例如，如果我们要为交通状况（路上的汽车数量）根据时间（分钟数据）建模。具体的分钟可能不重要，而时段如“上午”“下午”“傍晚”“夜间”“深夜”更有利于预测。如此建模更直观，也能避免过度拟合。
这里我们定义一个简单的、可复用的函数，轻松为任意变量分箱。

```
#分箱:

def binning(col, cut_points, labels=None):

  #Define min and max values:

  minval = col.min()

  maxval = col.max()

  #利用最大值和最小值创建分箱点的列表

  break_points = [minval] + cut_points + [maxval]

  #如果没有标签，则使用默认标签0 ... (n-1)

  if not labels:

    labels = range(len(cut_points)+1)

  #使用pandas的cut功能分箱

  colBin = pd.cut(col,bins=break_points,labels=labels,include_lowest=True)

  return colBin



#为年龄分箱:

cut_points = [90,140,190]

labels = ["low","medium","high","very high"]

data["LoanAmount_Bin"] = binning(data["LoanAmount"], cut_points, labels)

print pd.value_counts(data["LoanAmount_Bin"], sort=False)
```

想了解更多请阅读 Pandas Reference (cut)

##11 – 为分类变量编码

有时，我们会面对要改动分类变量的情况。原因可能是：

有些算法（如罗吉斯回归）要求所有输入项目是数字形式。所以分类变量常被编码为0, 1….(n-1)
有时同一个分类变量可能会有两种表现方式。如，温度可能被标记为“High”， “Medium”， “Low”，“H”， “low”。这里 “High” 和 “H”都代表同一类别。同理， “Low” 和“low”也是同一类别。但Python会把它们当作不同的类别。
一些类别的频数非常低，把它们归为一类是个好主意。

这里我们定义了一个函数，以字典的方式输入数值，用‘replace’函数进行编码。

```
#使用Pandas replace函数定义新函数：

def coding(col, codeDict):

  colCoded = pd.Series(col, copy=True)

  for key, value in codeDict.items():

    colCoded.replace(key, value, inplace=True)

  return colCoded

 ​

#把贷款状态LoanStatus编码为Y=1, N=0:

print 'Before Coding:'

print pd.value_counts(data["Loan_Status"])

data["Loan_Status_Coded"] = coding(data["Loan_Status"], {'N':0,'Y':1})

print '\nAfter Coding:'

print pd.value_counts(data["Loan_Status_Coded"])
```

编码前后计数不变，证明编码成功。
想了解更多请阅读 Pandas Reference (replace)

##12 – 在一个数据框的各行循环迭代

这不是一个常见的操作。但你总不想卡在这里吧？有时你会需要用一个for循环来处理每行。例如，一个常见的问题是变量处置不当。通常见于以下情况：

带数字的分类变量被当做数值。
（由于出错）带文字的数值变量被当做分类变量。

所以通常来说手动定义变量类型是个好主意。如我们检查各列的数据类型：

```
#检查当前数据类型：

data.dtypes
```

这里可以看到分类变量Credit_History被当作浮点数。对付这个问题的一个好办法是创建一个包含变量名和类型的csv文件。通过这种方法，我们可以定义一个函数来读取文件，并为每列指派数据类型。举例来说，我们创建了csv文件datatypes.csv。

```
#载入文件:

colTypes = pd.read_csv('datatypes.csv')

print colTypes
```

载入这个文件之后，我们能对每行迭代，把用‘type’列把数据类型指派到‘feature’ 列对应的项目。

```
#迭代每行，指派变量类型。

#注，astype用来指定变量类型。

for i, row in colTypes.iterrows(): #i: dataframe索引; row: 连续的每行  

  if row['feature']=="categorical":

    data[row['feature']]=data[row['feature']].astype(np.object)

  elif row['feature']=="continuous":

    data[row['feature']]=data[row['feature']].astype(np.float)

  print data.dtypes
  ```
  
现在信用记录这一列的类型已经成了‘object’ ，这在Pandas中代表分类变量。
想了解更多请阅读Pandas Reference (iterrows)

##结语

本文中我们介绍了多个可以帮助我们减轻数据探索、特征工程工作负担的函数。此外，我们也定义了一些函数，这些函数可以在不同的数据集上复用以获得相同效果。
原作者：AARSHAY JAIN 
翻译：王鹏宇
原文地址：
http://www.analyticsvidhya.com/blog/2016/01/12-pandas-techniques-python-data-manipulation/
