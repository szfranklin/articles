# Airbnb用机器学习帮助房东定价

Airbnb的动态定价功能可以向房东展示房子在某一价格可以出租出去的概率。如下图，绿色表示机率较高，红色表示机率较低。房东利用这个功能可以根据市场需求灵活机动地给他们的房子定价。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuXabG7VWdy9XTKvvLcpALAWt7uUUxticCt1JW6KOTCbbV1gcPOQaYyPaUJhWwYhvyGAIpB6iccvHAQ/0?wx_fmt=gif&tp=webp&wxfrom=5)

影响房屋出租价格的因素很多，例如季节，地区，评语等等，由于这些因素之间复杂的关联性，传统的算法模型产生的结果可能很难解释，无法准确地知道哪些因素影响较大。因此Airbnb隆重推出机器学习套件Aerosolve，更好地帮助房东理解具体是哪些因素在影响房屋可以出租出去的最优价格。首先假设价格和房屋需求之间的反向关系。如图1，红线是按常识建立的先验，即需求会随着价格的增加而减少。在Aerosolve中通过一个简单的文本配置可以把先验模型与大量的市场数据结合，得到图中黑线表示的模型。大量真实的市场数据完善了人们在原有资讯基础上的假设，帮助房东更好地了解市场。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuXabG7VWdy9XTKvvLcpALAkUJw3gpsUYQhKZ9Xd4iaGfx1yHSObnibOaPhBkZZtsUq0s3d6elAHaKw/640?wx_fmt=png&tp=webp&wxfrom=5)

影响出租价格的一个重要影响因素之一就是房屋所在的社区环境。在Aerosolve中利用K级空间树（Kd-Tree）按照房子的地点自动生成社区变量（这个算法会排除含有大片水域的地区，并且用多级方法平滑地区边界信息），然后按照社区之间的相似度为社区归类，同时建立层级模型，以决定不同社区在定价模型中的权重。这个模型可以根据点状数据（例如列表）或者多边形数据（例如搜索框）快速统计数据（例如房间被浏览的次数），如图2。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuXabG7VWdy9XTKvvLcpALAKma4icIwZsWwrSGthmOVl42L1BFuedrLR3HQM76ibYbPhSpUkjazSSAg/640?wx_fmt=png&tp=webp&wxfrom=5)
图2，Aerosolve洛杉矶中生成的社区

同时，在Aerosolve中还用图像分析算法来分析房屋的装饰如何影响出租概率，并且用两种训练数据来分别建立模型。如图3，左图是按专业摄影师的评分系统建立的模型，右图是住客评分系统建立的模型。从结果可以看出专业摄影师倾向有图画装饰并且明亮的起居室，而住客更喜欢温馨舒适的卧室。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuXabG7VWdy9XTKvvLcpALACicNb8kZC18R7DuJImgj1Pbd1ABxa1IcZric9QMXhtdbDVavTaGwcdtg/640?wx_fmt=png&tp=webp&wxfrom=5)
图3: 左图是依据专业摄影师评分模型得出的顺序，右图是住客评分模型得出的顺序。

当地活动也会影响房屋出租价格。如图4，可以看出在SXSW活动期间Austin的住房需求增加因此价格也随之增加。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuXabG7VWdy9XTKvvLcpALAHpdIEdG8qNEWxzcNrRaOVbrj9QhluLPYoUJjhc0Wsy8wJyesGd9sVA/640?wx_fmt=png&tp=webp&wxfrom=5)
图4: Austin的房屋季节性需求对价格的影响

房东得到的评语数量是否会对价格产生影响呢？如图5，在用三次多项式平滑因素曲线同时用狄拉克函数保留末端峰值后，得到的总评语数和三星评语数（最高为五星）对价格的影响。可以看出，零条评语和一条评语之间效果差异巨大，但是只要有了一条评语，更多的评语却不一定效果更好。同时，三星评价如果数量较少甚至会产生副作用。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWuXabG7VWdy9XTKvvLcpALAJic1yjZvbfmsE0fcLrE6np3tUw5k0miaJEibBHH2fjMSy1Dbqa23dbyXA/640?wx_fmt=png&tp=webp&wxfrom=5)
图5: 总评语数和三星评语数与价格之间的关系

影响房屋出租价格的因素很多，Airbnb利用大数据和机器学习方法，将人脑和机器完美结合，为房东出租价格提供最优建议，帮助这个共享经济得到更好的发挥。

- 翻译：DW
- 原文链接：http://nerds.airbnb.com/aerosolve/ 

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWsdBics1G9DJ7DmmxPh3TlpEiaHDISA36O9ePg9AyvDWAdBmNk1eOraCbh1esMPKFAyUjVedmrKwP9Q/640?wx_fmt=jpeg&tp=webp&wxfrom=5)
