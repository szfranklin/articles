# 流行音乐五十年的发展历程

自从 1958 年开始，每年的 12 月 Billboard 都会评选出当年最火热的 100 首流行单曲。本文主要利用图表来分析美国地区年度流行单曲的表现情况。

借助 R，我将入选最近 50 年(1965-2015)年度最佳流行单曲的歌曲信息整合到一个数据集中，你可以从我的 Github [项目](https://github.com/walkerkq/musiclyrics)中下载数据。(链接：https://github.com/walkerkq/musiclyrics)

## 获取歌曲数据

我主要从维基百科中的 Billboard 年度最热门 100 首单曲的网页中获取分析所需要的歌曲数据。从维基百科中获取的是年度数据，而不是每周的排名数据。但是许多歌手的数据是周度数据，我们需要将其转换为年度数据。

我利用 xml 和 RCurl 包从维基百科网页中抓取分析所需的歌曲和歌手的名字。接下来我利用第一步获取的列表数据从构建新的 URL 字段，并从其他网站中抓取歌词数据。比如对于网站 metrolyrics.com 而言，其网址链接为 metrolyrics.com/SONG-NAME-lyrics-ARTIST-NAME.html。如果无法从第一个网站中抓取到数据，我将依次从后续备选网站中爬取数据。最终结果显示，78.9% 的歌词数据来源于 metrolyrics.com 网站，15.7% 的歌词数据来源于 songlyrics.com 网站，1.8% 的歌词数据来源于 lyricsmode.com，大约 3.6% 的歌词数据无法获取。

最终的数据集中包含了 5100 条观测值，变量有歌曲排名(1-100)、歌曲名字、歌手名字、上榜年份、歌词和来源网站。虽然维基百科网页上的数据格式相当标准，但是数据中仍然存在一些噪声数据。需要注意的是，数据集中的歌词数据可能存在一些误差，我并没有修正它们。

## 数据探索分析
### 歌词中最频繁出现的词语

![Frequent Word](http://static.datartisan.com/upload/attachment/2016/06/0xI8zYwc.png)

### 58% 的歌手仅一次上榜

总的1989 名歌手中的 1154 名歌手(58%)仅上过一次榜。下表中的数据根据歌手名字进行汇总统计所得：

![Artists](	http://static.datartisan.com/upload/attachment/2016/06/JzaeFqql.png)

![per](	http://static.datartisan.com/upload/attachment/2016/06/CwH297kA.png)

### 职业生涯长短差异

我非常惊奇地发现职业生涯较短的歌手和高上榜率的歌手之间存在一定的相关性，比如蕾哈娜在 10 年的职业生涯中总共上榜了 28 次，所以接下来我打算探究歌手的职业生涯长短和平均每年上榜次数之间的关系，结果显示这两者之间存在负相关关系。歌手的职业生涯每增加一年，平均每年上榜次数将下降 94%。

*数据集中不包括甲壳虫乐队 1964 年的数据，所以实际上他们的职业生涯是 12 年。*

![career span](http://static.datartisan.com/upload/attachment/2016/06/rCa2EW4E.png)

![linear](http://static.datartisan.com/upload/attachment/2016/06/VCQXcc7i.png)

## 歌词的变化特点
### 歌曲的单词量和音乐长度逐年增长

数据集中的歌曲平均每首包含 332 个单词和 114 个不重复单词。歌曲的平均词频和词频方差逐年增长，这大概是因为随着时间的推移上榜歌曲具有更丰富的多样性。我对总词频和不重复的词频数做了对数处理，然后分别拟合线性回归模型，其中总词频的系数为 0.01873，而不重复词频数的系数为 0.0136。这说明随着时间的推移，每增加一年，上榜歌曲的总词频大约上涨 1.87%，不重复词频上涨 1.36%。

![two model](http://static.datartisan.com/upload/attachment/2016/06/JDUAEgFc.png)

歌曲词数的增长主要是由于歌曲的时间越来越长，歌曲长度从六十年代的 2.5 分钟增长到 4 分钟。

![hist](http://static.datartisan.com/upload/attachment/2016/06/J2GQigo5.png)

### 从 Boogie 到 Bitch: 每十年的歌词特征 

利用我之前[博文](http://kaylinwalker.com/text-mining-south-park/)(链接：http://kaylinwalker.com/text-mining-south-park/)中提到的对数似然统计量，我们可以识别出每十年间歌曲的风格。简而言之，歌曲语料库中出现越多的单词将拥有越高的似然统计量值。

从下图中我们可以很明显地看出：被重复收藏的歌曲会影响最终的计算结果。这引起了一个新的问题，似然统计量是否适用于歌词分析领域，单一的、高度重复的歌曲是否会扭曲分析结果。

![word](http://static.datartisan.com/upload/attachment/2016/06/ihpdcHiF.png)

## 需要考虑的事项

**Billborad 年度最佳 100 首歌曲评选政策的变化**

流行歌曲内容的变化至少可以部分归因于随着时间推移而发生改变的排名方法。Billboard 的评选政策将随着群众购买音乐的方式的变化而变化。

- 1958-1991: 排名由单曲销量和播放量所决定
- 1991: Billboard 开始利用 SoundScan 来搜集数字化的单曲销量数据
- 1998: Billboard 放弃参评歌曲必须以单曲发行的规定
- 2005: 网络下载数量(iTunes)被纳入评选体系中
- 2012: 流媒体点播(Spotify, Rhapsody)服务次数被纳入评选体系中
- 2013: 视频播放(YouTube)次数被纳入评选体系中

在 2005 年以前，消费者只能通过购买单曲唱片和点播歌曲来影响评选结果，如今的消费者拥有的更多的消费渠道，消费者可以通过观看视频、下载单曲或者购买唱片等方式来证明哪些才是流行歌曲。
![](http://static.datartisan.com/upload/attachment/2016/05/xKM5xlV4.png)

---

原文链接：http://kaylinwalker.com/50-years-of-pop-music/

原文作者：KAYLIN WALKER

译者：Fibears
---
