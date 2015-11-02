#【R】数说007电影
最新的詹姆斯邦德电影——大战幽灵危机——将在几周后上映。我认为根据影片综合质量（由影评家和观影群众的数据测算所得）和票房收入数据来快速回顾以前的邦德电影将是一件非常有趣的事情。更重要的是，我想要编写一个可以从网络上抓取并分析数据的 R 脚本。

我们尤其想要获得每部电影的以下几个数据：标题、上映年份、男主角、影评数据以及票房收入数据（经通货膨胀率调整）。

很不好意思，我没有考虑影片导演的数据。我可能对具体的细节一无所知，但是在萨姆·门德斯指导《007：大破天幕杀机》之前我甚至不关心每部电影的负责人是谁。

我主要通过维基百科网站获取相关的数据。

其实我曾计划自己做一系列的分析，但事实证明，维基百科已经汇集所有你想要的相关信息，所以你不需要做额外的无用功。

（备注：我最开始使用了好几个维基百科网页的数据，然后去 IMDB 搜索影评数据，最后去美联储网站下载通货膨胀率数据。事实证明，IMDB 不允许你从他们的网站上爬取数据，所以我努力寻找新的变通方法直到我遇到这个完美的维基百科网页。对了，如果你需要的话，这里有一些非官方的 API 接口。）

（http://www.omdbapi.com

https://stackoverflow.com/questions/1966503/does-imdb-provide-an-api）

以下是本文用到的一些软件包：

RCurl: 读取 HTTPS 数据

XML: 利用 readHTMLTable 函数分析维基百科网页数据

ggplot2: 绘图工具

那么，让我们从读取这些软件包开始吧：
![](http://static.datartisan.com/upload/attachment/2015/11/1wxfAHxm.png)
（https://en.wikipedia.org/wiki/List_of_James_Bond_films）

这是储存我们想要信息的维基百科网页，其中包含了两个独立的表格（分别是由 EON 公司和非 EON 公司制作的电影数据；后者包含了《007别传之皇家夜总会》）。让我们读取并整合数据吧。
![](http://static.datartisan.com/upload/attachment/2015/11/87MErJHs.png)
![](http://static.datartisan.com/upload/attachment/2015/11/DCB1lCHB.png)
此时，我们已经对数据进行了预处理，接下来让我们做一个快速的数据分析：
![](http://static.datartisan.com/upload/attachment/2015/11/ema5OXA2.png)
![](http://static.datartisan.com/upload/attachment/2015/11/OgmnUKGz.png)
接下来，我们将电影的票房收入和评分绘制在同一张散点图中。我们拥有足够多的数据来绘图，所以事不宜迟：
![](http://static.datartisan.com/upload/attachment/2015/11/43YzRAZf.png)
![](http://static.datartisan.com/upload/attachment/2015/11/cT1bha0Q.png)
##以下是几条我们得到的结论：

1.詹姆斯邦德系列电影有一个普遍的现象：如果该电影评分较高，那么其票房也比较高。然而奇怪的是，两部《Casino Royale》几乎分别落在图形的两个完全相反的端点处，其中大卫·尼文主演喜剧电影表现很糟糕，而丹尼尔·克雷格主演的严肃电影则表现的非常好。
2.怀才不遇的主角：提摩西·道尔顿和乔治·拉赞贝。他们主演的电影都非常给力，但是票房收入却如此低。
3.虽然《大破天幕杀机》、《霹雳弹》和《金手指》的口碑低于《诺博士》和《来自俄罗斯的爱情》，但是他们却带来了惊人的收益。对于邦德系列的电影来说，在《霹雳弹》（1967）上映之后，直到 47 年后的《大破天幕杀机》才超越了它的票房收入。
 

原文作者：José María Mateos Pérez

翻译：Fibears

原文链接: 
http://rinzewind.org/blog-en/2015/!.html
