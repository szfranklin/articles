## 引言

使用`stargazer`包可以将 R 构建的模型结果以`LATEX`、`HTML`和`ASCII`格式输出，方便我们生成标准格式的表格。

再结合`rmarkdown`，你就可以轻轻松松输出一篇优雅的文章啦~

本文“使用说明”部分主要参考`stargazer`的[说明文档][1]。（https://vectorf.github.io/）

## 安装及加载  

![80ed](http://static.datartisan.com/upload/attachment/2016/12/2e7Z3ter.png)  
## 使用说明

**注意：`stargazer`包的输出结果是相应格式的，例如输出`LATEX`格式，可以直接将结果粘贴进`WinEdt`等编辑器中输出表格。下文直接将结果以表格的形式展示。

我们使用 R 中自带的数据集`attitude`来简要说明`stargazer`包的用法。  

![81ed](http://static.datartisan.com/upload/attachment/2016/12/mWNCWbdh.png)    

`attitude`数据集中包括`rating`、`complaints`等八个变量：  

![82ed](http://static.datartisan.com/upload/attachment/2016/12/noHS17Fb.png)  

1.展示数据集的描述性分析和部分数据集内容  

![83ed](http://static.datartisan.com/upload/attachment/2016/12/GdZaKfQy.png)  

![84ed](	http://static.datartisan.com/upload/attachment/2016/12/qCOW9qcQ.png)  

![85ed](http://static.datartisan.com/upload/attachment/2016/12/Rr4OSziO.png)  

![86ed](http://static.datartisan.com/upload/attachment/2016/12/O5nnDnBH.png)  
怎么样？！是不是感觉还不错~

2.展示线性模型结果，并加上表名  

![87ed](http://static.datartisan.com/upload/attachment/2016/12/IDqK2Z9P.png)  
![88ed](http://static.datartisan.com/upload/attachment/2016/12/Cyqs7adZ.png)  
我们构建了两个线性模型和一个 Probit 模型，并将结果输出。

使用`title`参数将其命名为“Results”；

使用`align`参数使数字排列整齐。

3.对模型结果输出做部分调整：

- 更改变量名；
- 删除极大似然统计量、残差标准差、F统计量；
- 删除表中的空行。  
  

![89](http://static.datartisan.com/upload/attachment/2016/12/d6dDw3Wv.png)
![90](http://static.datartisan.com/upload/attachment/2016/12/xHMLhXyR.png)  
使用`dep.var.labels`和`covariate.lables`参数分别将因变量和自变量重命名为容易理解的形式；

使用`omit.stat`参数控制对数似然比（“LL”）、标准化残差（“ser”）和F统计量（“f”），这三个统计量不在输出结果中展示；

使用`no.space`参数将输出表格中的空行删去。

4.展示置信区间

![91ed](http://static.datartisan.com/upload/attachment/2016/12/A7yZJygT.png)
![92ed](http://static.datartisan.com/upload/attachment/2016/12/rUe11IM4.png)  
使用`ci`和`ci.level`参数展示90%的置信区间；

使用`single.row`参数使估计量与置信区间并排展示。

5.调整变量展示顺序，加上样本量，并移除其他统计量

![93ed](http://static.datartisan.com/upload/attachment/2016/12/U43WmleD.png)
![94ed](http://static.datartisan.com/upload/attachment/2016/12/u0K7suc0.png)  

使用`order`参数控制自变量展示的顺序，即将`learning`和`privileges`放在表的前两行；

使用`keep.stat`参数控制要展示的统计量，`keep.stat="n"`即只展示样本量的大小，并移除其他统计量。

6.以`ASCII`格式输出：

![95ed](http://static.datartisan.com/upload/attachment/2016/12/PISgEO1U.png)
![96ed](http://static.datartisan.com/upload/attachment/2016/12/gOA9wyCN.png)  
使用`type`参数控制以`ASCII`格式输出，还可以选择输出`HTML`格式。默认为`LATEX`格式。

相应地，将`type`参数分别设置为`text`、`html`、`latex`即可。

7.展示矩阵

![97ed](http://static.datartisan.com/upload/attachment/2016/12/I51bSmNL.png)
![98ed](http://static.datartisan.com/upload/attachment/2016/12/cC2cR66K.png)  

`stargazer`也可以用来展示向量、矩阵或者数据框的内容。

我们建立了`attitude`数据集中变量`rating`、`complaints`、`privileges`的相关系数矩阵，并展示出来。

8.自定义变量

我们使用`sandwich`包来计算异方差-稳健标准误，并将其与默认计算的标准差一同展示。

![99ed](http://static.datartisan.com/upload/attachment/2016/12/SqDKjnK7.png)
![00ed](http://static.datartisan.com/upload/attachment/2016/12/PC8L8NoB.png)  

## 与 `rmarkdown` 一起食用

`rmarkdown`包可直接在`RStudio`中编辑符合 `markdown`语法的文档，并兼容`LATEX`格式。而且可以直接输出成`HTML`、`pdf`等格式的文档。

因此，`stargazer`与`rmarkdown`一起食用，风味更佳~

首先，你需要在`Rstudio`中安装`rmarkdown`。

![01ed](http://static.datartisan.com/upload/attachment/2016/12/aszRiUM2.png)  

然后，就可以原先新建脚本的地方发现，可以新建一个`	R Markdown`文件啦。

在`rmarkdown`中，用如下所示的形式来表示代码块：

![02ed](http://static.datartisan.com/upload/attachment/2016/12/fh2ufQ8h.png)  

注意以下几点：

- 要加上`results='asis'`保证输出的是表格，而不是`LATEX`格式；
- 参数`align`失效，不能加上；
- 加上参数`header=F`，以避免输出关于包作者的一些信息。

其余用法与上述使用说明基本相同。这样就可以直接输出如上所示的表格了。

## 总结

`stargazer`用一行代码就可以解决模型结果输出成表格的问题，而且支持大量模型。具体可查看该包的[说明文档][1]。

最后，如果在你的文章中有使用了`stargazer`包。记得附注以下作者的信息哦。

> Hlavac, Marek (2015). stargazer: Well-Formatted Regression and Summary Statistics Tables. 
> R package version 5.2. http://CRAN.R-project.org/package=stargazer

------

> `stargazer`包的说明文档：https://cran.r-project.org/web/packages/stargazer/vignettes/stargazer.pdf
>
> 本文作者：[Vector][2]

[1]: https://cran.r-project.org/web/packages/stargazer/vignettes/stargazer.pdf	"stargezer.pdf"

[2]: https://vectorf.github.io/
