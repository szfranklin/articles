
# 数据可视化之美

信息膨胀的今天，数据可视化成为越来越重要的课题。基于可视化可洞察数据内部规律，挖掘高价值信息。本文在介绍可视化的重要性之后，将由浅入深介绍几种类型的可视化方法，带领大家体会数据可视化之美。

## 数据可视化的“疗效”：

精湛的可视化可以搭建起复杂数据与商业信息之间的桥梁。让我们来看几个巧妙的例子。

## 运动分析：

在过去的5年中，体育频道在信息传播方面取得了巨大的进步。通过可视化方法，观众可以在极短的时间内获得所需信息。例如，通过图1您可以轻松地获知运动员的最佳得分点。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm2398jia6hl37PLqRdm5Rbr3jibz3UeVNxgmPK2V2EsU6rzibibaXqc6lVywsQ/640?wx_fmt=png&tp=webp&wxfrom=5)


## 政治分析：

在图2 中，采用可视化的方法可以轻松分析印度各个州的选举情况。显然，NDA政党的支持度明显大于印度的其他组织。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239OLR5GJQ61VkMAuQUJEDxAibEKiaaC36MUvqsGColO9T42fR6gbibKwv2Q/640?wx_fmt=jpeg&tp=webp&wxfrom=5)

## 可视化常用方法：

不同分析目的所选择的可视化方法不同。用户目的一般可以分为四类：分布分析、对比分析、相关分析、构成分析。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239H1pOqqP1pJYXAIRgAUgibY5kHvntVPRLPLMIk835w1gEgNicXn2X5v2A/640?wx_fmt=jpeg&tp=webp&wxfrom=5)


## 分布分析：

在变量分析的初始阶段，通常需要了解变量的分布情况。变量一般分为两种类型：连续型和离散型。对于连续型变量，我们关注其中心水平、离散水平和异常值；对于离散型变量，我们关注其频率分布。用于描述变量分布的可视化图形有：直方图和箱线图。

直方图：直方图反映连续变量的分布情况。直方图需要确定的参数是数据分段的宽度。让我们来看下面的例子：

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239n3ib0FveV0jefso7FIHlaahpaU2cVxAb63gibDicdRoVuDXmqLibxFhOoA/640?wx_fmt=png&tp=webp&wxfrom=5)

图4数据来自于同一组乘客年龄分析数据，然而从右图中可以看出0-5岁的顾客明显多于6-16岁的顾客。但基于左图，我们却不能做这样的推断。因此，需结合分析目的，选择直方图带宽。

箱线图：用来展示变量从最小值到最大值的变化情况，并可用来识别异常值。它展示了最小值，下四分位数（Q1），中位数，上四分位数（Q3），最大值。在箱线图的上下两条横线代表的临界值之外的数据为异常值。

```
上临界值 = Q3 + 1.5 * (Q3-Q1)
下临界值 = Q1 – 1.5 * (Q3-Q1)
```

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239CgibWyRZAnFYO9BwF4ANGKFX3mYDdiaMzRyg5d5YI6Gf0rGDmkM79e5w/640?wx_fmt=png&tp=webp&wxfrom=5)


##比较分析：

比较分析常用来分析数据随时间或组别变化发生的变化。常见的可视化工具为柱状图和折线图。当我们比较不同组别的数据时多采用柱状图。分析交叉定量数据时，一般采用折线图。

组别差异图：

!()[http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239Rc3mOQqtNKSic9bFZUafD19qKJuQGvsq6fE2mHEQHqRAkqkK241MmsA/640?wx_fmt=png&tp=webp&wxfrom=5]

交叉定量数据比较：

!()[http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239CxIkIsu8sM2RsaWHwyEsBf3fZ6yJYHE9qGfqaZUhe9C7CO5bm4X6kw/640?wx_fmt=png&tp=webp&wxfrom=5]


在进行不同类别中不同变量对比时，还可以采用堆叠柱状图：

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239PAtslzsAdDcNJmQQw8mrvDeibql0LPLxnbgribEzA3rX2nAeo2NpVZXQ/640?wx_fmt=png&tp=webp&wxfrom=5)


当存在较多类别时，先分离不同的类别再进行比较是一种不错的选择。决策树是处理这种问题的一个比较好的工具。图8举例研究了学生玩板球（Cricket）的影响因素。图9左将学生按照性别分成两组，发现只有20%的女生玩板球，而玩板球男生占所有男生65%。图9中和图9右分别研究了身高和班级的影响。但身高和班级对是否玩板球的影响远远小于性别。


!()[http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239r1akK5kNnv2eMWtm4TtIibVGaCJcMfxZojtOp5KicWs09iam3Tenr3myA/640?wx_fmt=png&tp=webp&wxfrom=5]

## 相关分析

研究两个变量之间的相关关系在实际应用中至关重要。最常见的可视化方法之一为绘制散点图。它可以清晰的呈现出两个变量之间的关系。

散点图：

!()[http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239Ju0qBDTUr6364tuR6Z5jXauJ0tMSFyceRR8D1yctoqiaC7KExrXupiag/0?wx_fmt=gif&tp=webp&wxfrom=5]

我们还可以通过控制散点的形状和颜色等来加入第三个变量，绘制泡泡图（Bubble Chart）。

泡泡图：

!()[http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239fKPYj4pST3icfiaKeGtcCXHBdl9oBX4zic57Zu1edT5libwVnNbEaFGxmQ/640?wx_fmt=png&tp=webp&wxfrom=5]


当点数比较多时散点图会显得特别拥挤。在这种情况下，我们可以使每个点都稍微透明一些，这样那些点数比较密集的区域的颜色就会较深。

## 比较分析

在分析分类变量的分布情况时，可以结合饼状图和柱状图进行比较分析。

!()[http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm2398X5Dqx6yoZxsAA4lGqibbbjwicTd4ZyKA0BeJAXMe5pGIIZkcJ58iaxKw/640?wx_fmt=png&tp=webp&wxfrom=5]

当然堆叠柱状图也可以用来分析不同类别内的某两个变量的分布情况。

!()[http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239PAtslzsAdDcNJmQQw8mrvDeibql0LPLxnbgribEzA3rX2nAeo2NpVZXQ/640?wx_fmt=png&tp=webp&wxfrom=5]

## 高级可视化方法

截至目前为止，我们介绍的都是常用的可视化方法。接下来，本文将介绍一些高级的可视化方法。这些方法拉高了可视化讲故事的能力。

热图（HeatMap）：热图用颜色来表示像散点图、地理空间图、面积图上的数值。你可以针对最小值，最大值，中值点设置不同的颜色梯度来表示数据的变化。热图比传统的可视化方法包含更多的信息。

!()[http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm2393icN8DoRohZR2FaxGlTVFVBI8sqS2fGGxlXNCfTI1erO5FHP3zx1PrQ/640?wx_fmt=png&tp=webp&wxfrom=5]

地理图表：数据科学家开始在地理位置上绘制数据以期帮助一些政府或者组织更好的制定策略。这里我们依然可以应用颜色或者体积等代表一个变量的大小，这有些类似于之前在散点图上的操作，唯一的区别在于现在我们将这些颜色或者体积绘制在地图上。

采用地理图表进行分析有如下好处：

1. 可以清晰了解各个组织在地理位置上的分布情况。
2. 可以清楚了解每个城市数据的情况
3. 给策略制定者更好的直观感受

!()[http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239jNvXVTCGKcyqwhOkSbjIFFNxYzU0TJxT6GGfBWce13Y8ibU1VZrkaiaw/640?wx_fmt=png&tp=webp&wxfrom=5]

网格图：网格图采用2D的列表模式。在这种方法中有横纵两个测度。然后根据每个测度下的分类情况绘制网格，在绘制完网格后填充颜色以代表相应的数值。

!()[http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239yHITja24yAJBO0RmcAXJUyR3CWP38fIKaiaVluY5gwoevjONq1icPibpQ/640?wx_fmt=png&tp=webp&wxfrom=5]

举个例子来说，图16展示了不同工具之间分析和展示数据的能力。其中绿色代表专业、琥珀色代表中等，红色代表一般。从上图可以看出SAS在可视化、数据探索、回归、分类、机器学习方面都有很好的性能。（这里只是一个例子，几种分析工具之间的优劣还有待详细阐述——译者注。）

词云:词云是分析文本数据的可视化工具之一。它展示出一文本集中各个词汇出现频率的高低。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvZd7WeVvZdnQMsdUSsm239rRXwiaYCypLsLJQdNaoeNJYrllTrSUVERQLYY86Tb5yFPjyXIeSd8zg/640?wx_fmt=png&tp=webp&wxfrom=5)

- 原文作者：Sunil Ray
- 翻译：F.xy
- 编辑：Jude