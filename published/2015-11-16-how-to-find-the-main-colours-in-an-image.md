#【Python】图像主色的K-Means分析
在我先前的博文中，我介绍了如何从网站上抓取图片信息。如果说从网上抓取图片非常容易实现，那么如何对这些图像进行排序分类则稍微复杂一点。这个问题的关键在于，我们没有一套寻找图像主色的标准方法，不同的方法会产生不同的结果。

##颜色理论

比方说我们已经下载好图像信息，现在我们想要找出它的主色。如果你熟悉 Photoshop 的话，那么你一定认识代表主色分布的颜色直方图。Corad Chavez 写过一篇详细介绍颜色直方图的博文。我们可以直接从颜色直方图中看出下图的图像主色是蓝色和绿色。
![](http://static.datartisan.com/upload/attachment/2015/11/oCa08ipO.png)
我所使用的方法基于以下两个假设条件：
1.图像主色是由拥有多少该颜色的像素所决定的。
2.如果两个颜色的RGB成分之间的欧式距离比较大，我们则认为这两种颜色不同。

第一个假设听起来非常有道理，但问题是：在真实的图像中很难找到两个颜色相同的像素。如果一个图像是蓝色的，那么它将拥有数以千计的深浅不同的蓝色像素。第二个假设条件需要决定哪个颜色和蓝色比较相近。最常用的方法是观测该颜色的 RGB 三维空间坐标，即两个颜色之间的距离可以看成它们在三维空间中的直线距离。

让我们从加载图像开始吧：
![](http://static.datartisan.com/upload/attachment/2015/11/VUf6dxqx.png)
值得一提的是，改变图像的尺寸大小并不会改变其颜色的比例和分布，而且计算机处理小图像时效率比较高。第 5-10 行的代码目的是：在保持图像比例不变的前提下，改变图像的尺寸。

##步骤一：利用 K-Means 对颜色聚类分析

给定之前的两个假设条件，我们打算利用聚类来解决问题。我们拥有一大堆点的数据，我们需要根据它们的相似性对它们进行分组，同时我们还想找出每个组的中心点。 K-Means 算法是一种既简单又高效的聚类算法。我们可以利用 Scikit 库来实现它。
![](http://static.datartisan.com/upload/attachment/2015/11/FiFv1bHT.png)
第 4 行代码主要是将图像转换成包含像素的单列表文件，第 8 行代码才是真正的聚类分析过程。OpenCV 中的图像由 NumPy 的矩阵格式展现出来，比如一张包含 800*600 像素的图像，它由一个 800*600*3 的数组所构成，每个像素分别由 R, G, B 三个成分表示。K-Means 所做的事情就是对这些三维空间的 RGB 数据进行聚类分析。

##步骤二：根据像素点个数对聚类结果排序

接下来我们要做的事情是计算每个类群中包含多少像素点。我将利用 Adrian Rosebrock 的方法来处理这个问题。
![](http://static.datartisan.com/upload/attachment/2015/11/oov0UV46.png)
第 2 行代码用来寻找每个类群中所包含的像素点个数，而第 5-7 行代码用来对它们进行排序处理，其中第一类群中拥有最多的像素点。

##步骤三：聚类结果评估

本文的基础性教程到这里就结束了，不过只要你接触过 K-Means 算法，那么你肯定发现我跳过一个重要的步骤：如何选择类群个数。类群个数是 K-Means 算法中很重要的一个参数，本文中选择 k=3 。但是如果图像的聚类群数小于 2 时，结果将会是啥样的呢？此时要么是图像中只有两种主色，或者是在 RGB 空间中该算法无法较好地将颜色区分开来。如果你要探索 K-Means 算法的拟合情况，你可以利用 Silhouette 系数。该系数反映了聚类结果的得分情况，同时它还评估了两个东西：每个类群中像素点的紧密程度和类群的聚类效果。强大的 Scikit 同样可以算出 Silhouette 系数。
![](http://static.datartisan.com/upload/attachment/2015/11/gHH9ZaHK.png)
该系数的取值范围是  -1 到  +1 之间，其中后者表示聚类结果优异。我们通常多次试验 K-Means 算法从而选取最佳的类群个数。
![](http://static.datartisan.com/upload/attachment/2015/11/1uvcUFOO.png)
图像聚类的评估结果如下图所示，其中我们可以看出第一个聚类结果更好。
![](http://static.datartisan.com/upload/attachment/2015/11/BuMK1FqO.png)
对于色彩分明的图像来说，聚类效果特别好。比如，下图的聚类效果特别好，silhouette 系数表明最佳类群个数为 8。
![](http://static.datartisan.com/upload/attachment/2015/11/c9jPw2x3.png)

##结论

我们可以利用聚类算法找出图像主色，其中最常用的模型是简便又高效的 K-Means 算法。但需要注意的是，我们需要预先压缩图像的尺寸且找出最佳类群的个数。

本文的例子中，我们在 RGB 空间中对颜色进行聚类分析。其他文章认为：在 LAB 空间中分析问题可以得到更好的结果。如果你对这个问题感兴趣的话，你可以查阅 Eddie Bell 的相关文章。

参考资料：

http://creativepro.com/better-tools-for-tones-why-i-don-t-look-the-histogram/

http://www.pyimagesearch.com/2014/05/26/opencv-python-k-means-color-clustering/

 

原文链接:

http://www.alanzucconi.com/2015/05/24/how-to-find-the-main-colours-in-an-image/

原文作者：Alan Zuccoi

翻译：Fibears
