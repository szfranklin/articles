##Github上Pandas,Numpy和 Scipy三个库中20个最常用的函数

几个月前，我看到一篇博客中列出了 Github 网站上 Python 常用库中使用频率最高的一些函数/模块。我在这个基础上做了可视化处理，并撰写了每个库中使用频率前十的函数示例。其中本文中只包含了部分示例，完整的示例可以参见我的 Github。  

首先我利用 requests 和 BeautifulSoup 从原始博客中爬取相关的数据，然后利用 matplotlib 和 seaborn 来绘制条形图，其中函数的排序由包含该函数的资源库(Repositories)数目所决定。比如，虽然 pd.Timestamp 的总频次特别高，但是该函数仅在少量的资源库中出现，所以它的排序相对靠后。  

###Pandas
![](http://static.datartisan.com/upload/attachment/2016/09/Pzig7ZuB.png)  

####DataFrame: 创建一个 dataframe 对象  
![](http://static.datartisan.com/upload/attachment/2016/09/8atzLrgD.png)  

####merge：联结两个 dataframe 
![](http://static.datartisan.com/upload/attachment/2016/09/PMi2Axd9.png)  

![](http://static.datartisan.com/upload/attachment/2016/09/VMHIipX3.png)   

####Numpy  
![](http://static.datartisan.com/upload/attachment/2016/09/lq2bRpMV.png)  

####arange: 创建某个区间内等间距的序列数组  
![](http://static.datartisan.com/upload/attachment/2016/09/E5iQmYas.png) 

####mean: 沿着某个轴向计算列表/数组中所有数据的平均数  
![](http://static.datartisan.com/upload/attachment/2016/09/3cpYhjve.png) 

####Scipy  
![](http://static.datartisan.com/upload/attachment/2016/09/jI7nztnY.png)  

####stats: 常用的统计函数或分布函数  
![](http://static.datartisan.com/upload/attachment/2016/09/9gL66PfQ.png) 
 
![](http://static.datartisan.com/upload/attachment/2016/09/Uwk2uoZt.png)  

####linalg: 常用的线性代数函数，如逆矩阵(linalg.inv)、行列式(linalg.det)
![](http://static.datartisan.com/upload/attachment/2016/09/zF4KBDpc.png)   

####interpolate: 样条函数和插值函数  

![](http://static.datartisan.com/upload/attachment/2016/09/JV2eQaNQ.png)

![](http://static.datartisan.com/upload/attachment/2016/09/PU94noGG.png)  

####signal: 包含信号处理工具  
![](http://static.datartisan.com/upload/attachment/2016/09/VGnfFbAL.png) 
![](http://static.datartisan.com/upload/attachment/2016/09/lYbfhNME.png)  

####misc: misc.imread 和 misc.imsave 分别用于读取和保存图像数据
![](http://static.datartisan.com/upload/attachment/2016/09/SuTMvM2G.png)

![](http://static.datartisan.com/upload/attachment/2016/09/pbpKm0Ei.png)  

最后谢谢各位的阅读，你可以在我的 Github中看到完整的函数示例。

![](http://static.datartisan.com/upload/attachment/2016/05/xKM5xlV4.png)
***
原文链接：https://galeascience.wordpress.com/2016/08/10/top-10-pandas-numpy-and-scipy-functions-on-github/  
原文作者：Alexander Galea  
译者：Fibears  


