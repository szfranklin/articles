#GLM - Chapter 5  
`ggTimeSeries` 包提供了一个新颖的时间序列可视化方法。它是基于 `ggplot2` 包开发的，它同时还提供了一系列 `geom` 方法和一些已经打包好的绘图函数。接下来我们将举几个例来说明这个包。

我们可以利用 `devtools` 来安装这个包: `devtools::install_github('Ather-Energy/ggTimeSeries')`。

##直线图表
随着时间的推移，物联网设备生成了许多序列数据，它们也被称之为时间序列数据。对于这些时间序列数据，传统的处理方法是将它们绘制在同一张直线图表中。直线图表于 18 世纪初被提出，该方法可用于观测时间序列的趋势变化情况和对比不同时间序列之间的变化情况，它不仅易于绘制而且通俗易懂，这是一个非常棒的可视化方法。如今它的使用范围非常广，从医院的心跳监测器到证券交易员的交易软件都能看到它的身影。  
![](http://static.datartisan.com/upload/attachment/2016/06/B1NkdkeN.png)  
##替代方法  
然而，在某些情况下，数据科学家的要求变得更加苛刻和具体。以下列出了 5 个数据科学家可能会用到五种替代方法。它们主要利用了 `geom` 方法或者提前打包好的函数。

在介绍例子之前，我们首先需要设置下主题的参数:  
![](http://static.datartisan.com/upload/attachment/2016/06/zYyJ6ho4.png)
  
##日历热图
我们可以利用 `stat_calendar_heatmap` 和 `ggplot_calendar_heatmap` 方法来绘制日历热图。

日历热图是一个将逐日数据可视化的好方法。它的构建方法决定了我们可以非常容易地利用它来监测周数据、月度数据或者季度数据。  
![](http://static.datartisan.com/upload/attachment/2016/06/00KLhMw4.png)  

![](http://static.datartisan.com/upload/attachment/2016/06/Zilgrc0f.png)  

![](http://static.datartisan.com/upload/attachment/2016/06/oHl9fbPu.png)	

![](http://static.datartisan.com/upload/attachment/2016/06/helVUk6E.png)  

##地平线图
我们可以利用 `stat_horizon` 和 `ggplot_horizon` 函数来绘制地平线图。

试想一下，我们有一张被分割成多个相同高度的面积图。如果你将小块的图叠在一起并用颜色标记出不同的色块，那么你就得到一张地平线图了。当数据集中的 y 值变化范围特别大且具有偏态分布时，利用地平线图可以在不失去上下文信息的情况下标注出离群值。  
![](	http://static.datartisan.com/upload/attachment/2016/06/5utvOcMK.png)

![](http://static.datartisan.com/upload/attachment/2016/06/cZu4KqvF.png)

##蒸汽图
我们可以利用 `stat_steamgraph` 函数来绘制蒸汽图。

蒸汽图是堆积面积图的美化版。它通过将方差较大的组置于边缘，将方差较小的组置于中心从而达到突出数据变化情况的效果。而且蒸汽图的图形是中心对称的，这使得我们可以更加容易地比较不同序列随时间变化的情况。

![](http://static.datartisan.com/upload/attachment/2016/06/xX0SUQDb.png)

![](http://static.datartisan.com/upload/attachment/2016/06/2bdsaEdg.png)

##水流图
我们可以利用 `stat_waterfall` 和 `ggplot_waterfall` 函数来绘制水流图。

水流图不仅仅反应了数值大小，还展示了数据的变化方向。  

![](http://static.datartisan.com/upload/attachment/2016/06/HeihMZxW.png)  
![](http://static.datartisan.com/upload/attachment/2016/06/fqNzi6L2.png)

##点数图
我们可以利用 `stat_occurrence` 来绘制点数图。

这是一个常用的信息图表。对于某些特定情况，用事件发生的次数点图来替代数值反而能带来更佳的阅读效果。
![](http://static.datartisan.com/upload/attachment/2016/06/ZmfPD2r2.png)
![](http://static.datartisan.com/upload/attachment/2016/06/fxOLSsKV.png)

![](http://static.datartisan.com/upload/attachment/2016/05/xKM5xlV4.png)
***
原文链接: https://github.com/Ather-Energy/ggTimeSeries

原文作者: iyogeshjoshi

译者: Fibears



