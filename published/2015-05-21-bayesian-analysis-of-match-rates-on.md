# 贝叶斯方法教你玩转社交网站

520刚刚过去，如果你还没有可倾诉衷肠的目标人物，请不要灰心。现如今有各种社交应用可以给你神助攻，让你在下一个520，214，1314等等数字来临之日可以欢度节日。美国一个社交APP Tinder，就在各种单身人群中非常流行。Tinder会给你推送用户的照片主页，如果你喜欢的话就右滑，不喜欢就左滑，当两个用户同时右滑时，就配对成功了，然后他们就可以开始短信交流。
 
最近，几个单身学霸用Tinder做了个试验。一个男生和一个女生分别右滑了100个Tinder用户照片主页，看看他们能有多少配对成功。结果差异却很大：女生得到了90个配对，而男生却只有1个！类似的性别差异在其他试验中也有所体现，一个类似的试验中女生得到20%的配对而男生只有7％的配对。为什么男生在Tinder上有这么差的配对率呢？原因可能很多，其中一个可能是女生使用Tinder的频率相对较低，活跃度没有男生高。于是学霸们发威，开始用贝叶斯法分析Tinder整体用户活跃度是如何影响某一用户配对率的。
 
假设某一用户得到的实际回复率是p，其真实魅力值（即理论回复率）是r，APP用户活跃度是q，且p＝r＊q。也就是说，在我们所滑过的100个用户中，只有q％会看到我们的主页，更别说也同时右滑了。因此，如果我们想知道自己理论回复率r的话，得到用户活跃度q可以帮助我们估计真实魅力值。
 
Tinder用户的活跃度不尽相同，有的人无时无刻不在追踪主页的最新更新，也有的人可能几天或者几周都不去查看他们的主页。而从Tinder上我们能得到唯一数据就是用户最新一次的活跃时间。因此，在这里我们假设Tinder用户的平均活跃周期服从中心为10小时的正态分布。但是，即便一个用户平均每10小时使用一次Tinder，他可能有时候1小时就使用一次，有时候也许1周才使用一次。为了考虑同一用户活跃度的变化性，我们假设其平均活跃周期的方差服从指数分布。由于有的用户也许10小时或者更长的时间区间内都没有使用Tinder，且我们能观察到的最新一次活跃时间只是时间区间中的一点，因此时间区间越长我们能观察到用户活跃的可能性就越大，这导致我们的曲线分布向右偏倾。为了考虑时间区间引起的偏差，我们用时间区间值去标准化概率值，如图一。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvUffJPPe59sN5IJ72QFmXxXGTmKxyFubk6Ay1cJKBGibCmgebGtOgRB0rSKuToHzJvLKF4qsmcQKw/640?wx_fmt=png&tp=webp&wxfrom=5)

<div class="text-center">
图一：平均活跃周期为10小时的Tinder用户活跃度的指数概率密度函数（用时间区间标准化）
</div>

我们的数据记录了最新一次活跃时间，但下一次的用户活跃时间却依然未知，一个15分钟之前活跃的用户再次活跃的时间可能是10分钟后，也可能是10天后。因此在两次活跃的时间区间内，我们认为每一个时间点用户活跃的概率都是相同的。结合某一时间区间用户活跃概率和某一时间点活跃概率，我们得到最新活跃时间的分布，如图二。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvUffJPPe59sN5IJ72QFmXx9HSg506RTQn2mNPTVCpx1WxMpibheYicAj1T1ibeZJHicPyCgZB34Zbv2g/640?wx_fmt=png&tp=webp&wxfrom=5)

<div class="text-center">
图二：平均活跃周期为10小时的Tinder用户最新活跃时间的指数累计分布函数 
</div>

我们对100个用户进行了取样，记录了他们的活跃时间和次数。根据平均活跃度的先验分布和以及相应的最新活跃时间的分布，我们得到了如图三的后验分布（浅蓝色）。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvUffJPPe59sN5IJ72QFmXxhETiaMiaaBianoPcuCm3vf8rJSBx9XyhVUA3n8Z7QsWdIqsInIPguTwSA/640?wx_fmt=png&tp=webp&wxfrom=5)

<div class="text-center">
图三：平均活跃周期为10小时的Tinder用户活跃度的指数概率密度函数（按时间区间标准化）
</div>

后验分布和先验分布在同一时间点达到峰值，但是后验分布标准方差更小，这可能是和数据量增加有关，也可能表明Tinder建立了算法只推送相对较活跃的用户（否则，对于每24小时或更久才使用一次Tinder的用户我们应该会看到分布曲线有较长的尾部）。我们可以建立模型预测一定时间区间内多少用户会看到我们的主页。在这里，为了模型的简化，我们假设用户一登陆就会看到我们的主页，忽略他们活跃时的其他时间消耗以及Tinder自身的推送算法。我们先得到某个时间区间的平均回复率，然后用该平均回复率的似然值标准化概率，就得到了活跃度的分布q，即某个时间区间内活跃用户的比例，如图四。

![](http://mmbiz.qpic.cn/mmbiz/ghbI8QDvgWvUffJPPe59sN5IJ72QFmXxFl8sGoe5dNYlKFagJBIDSibjscybtN2WEZHEDCHyDbsev3YPT0olWXw/640?wx_fmt=png&tp=webp&wxfrom=5)

<div class="text-center">
图四：时间区间为5小时或10小时的用户活跃度q的分布 
</div>

有了用户活跃度q的分布，再结合实际观察到的回复率p，依据公式p＝r＊q我们可以进一步得出理论回复率r。结果可以看出，你的真实魅力值（即理论回复率）总是比你实际得到的回复率要高。所以好消息是，别因社交网站的较低回复率而灰心，你比你在APP上显示的更有魅力！

- 本文内容翻译并编辑自 Bayesian analysis of match rates on Tinder, by Ankur Das and Mason del Rosario
- 原文链接：http://allendowney.blogspot.com/2015/02/bayesian-analysis-of-match-rates-on.html
- 翻译：DW