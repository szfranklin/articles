假如你是谷歌码农，需创建一个谷歌搜索，你会采取怎样的基本原则来构建搜索引擎？如果你想采用词频或者TF-IDF 作为基本框架，那么你将面临如下窘境：

一用户输入查询“Harvard Business School”。他期待看到的是“http://www.harvard.edu/”，基于你的词频或者TF-IDF框架，计算机将尝试去寻找“Harvard”出现次数最多的网页，而将“Business”和“School”作为常用词处理。可Harvard 大学的官网可能并不会出现很多次“Harvard”，而像商学院网站上的评论或文章更容易出现“Harvard”这个词汇，这些网站会自动排在Harvard大学官网之前。那么显然用户会给你一个大大的差评。

谷歌究竟是采用什么方法对网页进行排序呢？没错就是“PageRank”。根据维基百科的定义，PageRank是一种由搜索引擎根据网页之间相互的超链接来计算网页排名的方法。其潜在假设为，一个网页与其他网页的相互链接越多，该网页的重要程序可能会越大。

为阐述该方法的具体应用，我们举一个简单的例子。假设一个只有4页面的网站，各网页之间相互联系情况见图1。其中蓝色框代表网页。蓝色框中黑色意大利斜体单词为网页之间的相互连接。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWt6XUBl8ib4NWRuQR3wLFdjmzdbicicrSQmXBrlSt0jbjYAVPcPgFsGAGkTTNY6kKCfUK3VzKR9nsBJw/640?wx_fmt=png&tp=webp&wxfrom=5)

显然对于名为‘Tavish’的网站来说，有3条指向其他网站的链接，为了清楚的观察各个网页之间的联系，我们绘制如下网页关系图。图2中显示，链接到“Kunal Jain”网页数目为3，为被链接次数最多的网页，所以在这四个网页中“Kunal Jain”为最重要的网站。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWt6XUBl8ib4NWRuQR3wLFdjmHRicZVMpMHWg80ACPicq2kgIGx29geOR0uC3D8PkQlu6gOhXbnUyXKGg/640?wx_fmt=png&tp=webp&wxfrom=5)

有了直观认识，接下来介绍一下PageRank 的数学支撑。为了得到各网页的最终排名，首先构造网页的链接矩阵，定义该矩阵为A。矩阵的每一列表示链接离开网页，每一行表示链接进入网页。例如从“Kunal Jain”（简称KJ）出发，可以链接到“IIT”和“Analytics Vidhya”，故矩阵的第一列为（0，0，0.5，0.5）。值得注意的是矩阵的每列和为1。现在假设有一个机器将自动访问这四个网页，计算在任一时刻该机器在每个网页上停留的概率。根据马尔科夫链知识，我们可以构造函数AX=X来求得该概率，其中A为网页链接矩阵。X 是机器停留在每个网页上的概率。通过计算得到，机器在“Kunal Jain”，“Tavish”, 
“Analytics Vidhya”，“IIT”上停留的概率分别为（0.4,0.12,0.24,0.24）。这也说明了“Kunal Jain”网页的重要性。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWt6XUBl8ib4NWRuQR3wLFdjmwpN62satciaVxq64cFLM9ZKicDzyzHKEw3qpoCxmQlCD8tjB23ibmPRsA/640?wx_fmt=png&tp=webp&wxfrom=5)

以上是PageRank 的主要思想，但这种方法依然存在一些问题。假设只有两个网页A和B，A可以链接到B，但B并无外链。采用之前提到的方法会得到A 和B的重要性相同的结论，这显然违背我们的直观感觉——B比A更重要。为解决这个问题，我们需要引入跳转到其他网页的常数概率alpha。修正后的模型变为：

`(1-alpha) * A * X + alpha * b = X`

其中，b为常数单位列矩阵。在大多是情况下，alpha的取值为0.15。

除搜索引擎外，PageRank的思想有很多其他的应用场景：

1. 确定个人在社交媒体中的重要性：在社交媒体中很少被提及的领域之一为网络交互信息。通过研究我们可以估计用户在社交媒体中所处的地位，并根据用户地位考虑如何设置服务水平。Page Rank 算法将大有可为。
2. 制药行业的欺诈检验：包括美国在内的很多国家都不遗余力的打击医疗欺诈行为，采用Page Rank可以标注欺诈行为。
3. 了解任何机器语言中程序包的重要性：Page Rank 算法可以被用来理解R语言或者Python 语言中包的层次。

 
- 翻译：F. xy
- 编辑：Ying and Jude
